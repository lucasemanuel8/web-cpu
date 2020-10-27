<template>
    <div class="instruction">
        <div class="row justify-content-center align-items-center">
            <div class="col">
                <h1 class="title">WEB-CPU</h1>
                <h2>Simulador de processador</h2>
                <p class="how-to">
                    Como usar: <a href="https://github.com/lucasemanuel/web-cpu#como-usar" class="link">Leia-me</a>
                </p>
                <span class="list-bits">
                    <a href="#" v-on:click="setBits(4)" class="select-bits active" id="4-bits">4 bits</a>
                    <span class="separator"/>
                    <a href="#" v-on:click="setBits(8)" class="select-bits" id="8-bits">8 bits</a>
                    <span class="separator"/>
                    <a href="#" v-on:click="setBits(16)" class="select-bits" id="16-bits">16 bits</a>
                </span>
                <form>
                    <div class="form-group">
                        <label for="instruction">Instrução:</label>
                        <select v-model="instruction" id="instruction" class="form-control">
                            <option value="AND">AND</option>
                            <option value="NAND">NAND</option>
                            <option value="OR">OR</option>
                            <option value="XOR">XOR</option>
                            <option value="NOR">NOR</option>
                            <option value="XNOR">XNOR</option>
                            <option value="ADD">ADD</option>
                            <option value="COMP">Complemento de 2</option>
                        </select>
                    </div>
                    <div class="form-group" id="form_reg_ax">
                        <label for="registrador_ax">Registrador AX:</label>
                        <input v-model="reg_ax" type="text" class="reg form-control" maxlength="4" id="registrador_ax">
                    </div>
                    <div class="form-group" id="form_reg_bx">
                        <label for="registrador_bx">Registrador BX:</label>
                        <input v-model="reg_bx" type="text" class="reg form-control" maxlength="4" id="registrador_bx" pattern="[0-1]">
                    </div>
                    <button v-on:click="select_instruction" type="button" class="btn btn-dark btn-block font-weight-bold">EXEC</button>
                </form>
                <div class="d-none row-result">
                    <span class="result text-center">
                        Resultado <span v-if="carry !== ''"> - Carry [{{ this.carry }}]</span> 
                        <span class="result content-copy" v-on:dblclick="copyContent"> {{ result }}</span>
                    </span>
                    <p class="copy">Para copiar click duas vezes no resultado.</p>
                </div>
                <h3 class="author">Criado por: <a href="https://github.com/lucasemanuel" class="link">@lucasemanuel</a></h3>
            </div>
        </div>

    </div>
</template>

<script>
export default {
    name: 'Instruction',
    data() {
        return {
            reg_ax: '',
            reg_bx: '',
            result: '',
            instruction: '',
            bits: 4,
            carry: '',
        };
    },
    mounted() {
        document.querySelector("#instruction").focus();
        var input_reg_bx = document.querySelector("#form_reg_bx");
        document.querySelector("#instruction").addEventListener("input", function () {
            if(this.value.toLowerCase() == "comp")
                input_reg_bx.classList.add("d-none");
            else
                input_reg_bx.classList.remove("d-none");
        });
    },
    methods: {
        select_instruction() {
            try {
                this.carry = '';
                this.validate();
                this[this.instruction.toLowerCase()]();
                document.querySelector(".row-result").classList.remove("d-none");
            } catch (e) {
                if(e instanceof TypeError)
                    alert("Instrução Invalida!");
                else
                    alert(e);
            }
        },
        xor() {
            this.result = this.compare("^");
        },
        and() {
            this.result = this.compare("&");
        },
        nand() { 
            this.result = this.invertBits(this.compare("&"));
        },
        or() { 
            this.result = this.compare("|");
        },
        nor() { 
            this.result = this.invertBits(this.compare("|"));
        },
        xnor() {
            this.result = this.invertBits(this.compare("^"));
        },
        add() {
            var reg_bx_r = this.reg_bx.split('').reverse().join('');
            var reg_ax_r = this.reg_ax.split('').reverse().join('');
            var carry = '';
            this.result = (reg_ax_r.split('').map(function (n, i) {
                if (carry === '') carry = 0;
                var add = Number(n) + Number(reg_bx_r[i]) + Number(carry);
                if (add == 1 || add == 0) {
                    carry = 0;
                } else if (add == 2) {
                    carry = 1;
                    add = 0;
                } else if (add == 3) {
                    carry = 1;
                    add = 1;
                }
                return add;
            }).join('')).split('').reverse().join('');
            this.carry = carry;
        },
        comp() {
            var reg_ax_i = this.invertBits(this.reg_ax.split('').reverse().join(''));
            var one = 1;
            this.result = (reg_ax_i.split('').map(function (n) {
                if (one === 1) {
                    if (n == 1) {
                        n = 0;
                    } else if (n == 0) {
                        n = 1;
                        one = 0;
                    }
                }
                return n;
            }).join('')).split('').reverse().join('');
        },
        validate() {
            this.reg_ax = this.validateNumber(this.reg_ax);
            this.reg_bx = this.validateNumber(this.reg_bx);
        },
        compare(operator) {
            var reg_bx = this.reg_bx;
            return this.reg_ax.split('').map(function (n, i) {
                return eval(n + operator + reg_bx[i]);
            }).join('');
        },
        validateNumber(n) {
            this.isBinary(n);
            if (n.length < this.bits) 
                return this.setZerosInLeft(n);     
            return n;
        },
        isBinary(n) {
            n.split('').map(function (n) {
                if (!n.match(/[0-1]/)) throw "Apenas números binários!!!";
            });
        },
        setZerosInLeft(n) {
            var len = n.length;
            var x = n;
            for (; len < this.bits; len++)
                x = "0" + x.toString();
            return x;
        },
        setBits(bits) {
            this.bits = bits;
            document.querySelectorAll(".reg").forEach(function (input) {
                input.maxLength = bits;
            });
            this.setClassActive(bits);
            this.clearInputs();
            document.querySelector(".row-result").classList.add("d-none");
        },
        setClassActive(bits) {
            document.querySelector(".active").classList.remove("active");
            document.getElementById(bits + "-bits").classList.add("active");
        },
        clearInputs() {
            this.reg_bx = '';
            this.reg_ax = '';
            this.result = '';
            this.carry = '';
            document.querySelector("#instruction").focus();
        },
        invertBits(n) {
            return n.split('').map(function (x) {
                return (1 - x).toString();
            }).join('');
        },
        copyContent() {
            let content = document.querySelector("span.content-copy").textContent;
            var textArea = document.createElement("textarea");
            textArea.value = content;
            document.body.appendChild(textArea);
            textArea.select();
            document.execCommand("Copy");
            textArea.remove();
        },
    },
}
</script>

<style>
.row {
    height: 100vh;
}
.row .col {
    max-width: 480px;
}

.how-to {
    font-size: 1.6rem;
}

h1.title {
    font-size: 4em;
    font-weight: 600;
    margin-bottom: 0;
}

.list-bits {
    font-size: 1.6rem;
    font-weight: 600;
}

.list-bits a {
    text-decoration: none;
}

.list-bits a:active, .list-bits a:hover, .active {
    color: #373a40;
}

.separator:after {
    content: '|';
    margin: auto 1rem;
}

form {
    margin-top: 1rem;
}

form label {
    font-size: 1.2rem;
}

.row-result {
    font-size: 1.6rem;
    margin-top: 1rem;
}

.author {
    font-size: 1.6rem;
    margin-top: 1rem;
}

span .result {
    font-weight: bold;
    font-size: 1.6rem;
}

p.copy {
    font-size: 1rem;
}

/* .instruction {
    margin-top: 2em;
}

.instruction {
    padding-bottom: 2em;
}

.btn-dark {
    color: #fff;
    background-color: #1d2124;
    border-color: #1d2124;
}

.btn-dark:hover {
    color: #fff;
    background-color: #23272b;
    border-color: #23272b;
}


ul {
    padding-inline-start: 0px;
    margin-bottom: 0.5rem;
}

ul li {
    list-style: none;
    display: inline;
}

h3 {
    font-size: 1.4em;
}

h3 a {
    font-size: 1em;
}

h3 a:hover {
    font-size: 1.2em;
}

h3 ul li {
    text-align: center;
    display: block;
}

.form-group {
    margin: 0 0;
}

.list-bits li {
    margin-right: 2em;
}

.list-bits li:first-child {
    margin-left: 2em;
}

.list-bits li:not(:last-child):after {
    content: "|";
    margin-left: 2em;
}

.active {
    color: #121518;
}

a {
    color: #fff;
    font-size: 1.6em;
    transition: .4s ease-in-out;
}

a:hover {
    font-size: 1.8em;
    color:#680c15;
    text-decoration: none;
}

a.link {
    text-decoration: underline;
    color: rgb(50, 0, 0);
}

input.form-control {
    color: #fff;
    background-color: rgba(0, 0, 0, .3);
}

input.form-control:focus {
    background-color: rgba(255, 255, 255, .1);
    border-color: #fff;
    -webkit-box-shadow: 0 0 0 0.1rem #fff;
    box-shadow: 0 0 0 0.1rem #fff;
    color: #fff;
    text-transform: uppercase;
}

input {
    text-transform: uppercase;
    background-color: rgba(0, 0, 0, .3);
    border-radius: .4em;
    padding: 0 .4em;
    color: #fff;
    text-align: center;
    margin-bottom: .4em;
}

button {
    margin-top: 1em;
}

.result {
    text-transform: uppercase;
    font-size: 1.4em;
    display: block;
}

p {
    margin: 0;
}

p.result {
    margin-top: .6em;
}

span.result {
    font-size: 2em;
    cursor: pointer;
}

h3.author {
    margin-top: .6em;
} */

</style>