<template>
    <div class="instruction row">
        <div class="col-sm">
        <h1 class="title">WEB CPU</h1>
        <h4>Trabalho de Arquitetura e Organização de Computadores</h4>
        <h5>
            <a href="https://github.com/lucasemanuel8" class="link">@lucasemanuel8/web-cpu</a>
        </h5>
        <ul class="list-bits">
            <li><a href="#" v-on:click="setBits(4)" class="select-bits active" id="4-bits">4 bits</a></li>
            <li><a href="#" v-on:click="setBits(8)" class="select-bits" id="8-bits">8 bits</a></li>
            <li><a href="#" v-on:click="setBits(16)" class="select-bits" id="16-bits">16 bits</a></li>
        </ul>
        <form>
            <div class="form-row">
                <div class="col-md-4 offset-md-4">
                    <div class="form-group">
                        <label for="instruction">Instrução:</label>
                        <input v-model="instruction" class="form-control" type="text" maxLength="3" pattern="/[0-1]/">
                    </div>
                    <div class="form-group">
                        <label for="registrador_ax">Registrador AX:</label>
                        <input v-model="reg_ax" type="text" class="reg form-control" maxlength="4">
                    </div>
                    <div class="form-group">
                        <label for="registrador_bx">Registrador BX:</label>
                        <input v-model="reg_bx" type="text" class="reg form-control" maxlength="4">
                    </div>
                    <button v-on:click="select_instruction" type="button" class="btn btn-danger btn-block">START</button>
                </div>
            </div>
        </form>
        <p class="result" v-if="result">
            Resultado 
            <span class="result">{{ result }}</span>
        </p>
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
        };
    },
    mounted() {
    },
    methods: {
        select_instruction() {
            try {
                this.validate();
                this[this.instruction.toLowerCase()]();
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
        validate() {
            this.reg_ax = this.verifyNumber(this.reg_ax);
            this.reg_bx = this.verifyNumber(this.reg_bx);
        },
        compare(operator) {
            var reg_bx = this.reg_bx;
            return this.reg_ax.split('').map(function (n, i) {
                return eval(n + operator + reg_bx[i]);
            }).join('');
        },
        verifyNumber(n) {
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
        },
        setClassActive(bits) {
            document.querySelector(".active").classList.remove("active");
            document.getElementById(bits + "-bits").classList.add("active");
        },
        clearInputs() {
            this.reg_bx = '';
            this.reg_ax = '';
            this.result = '';
        },
        invertBits(n) {
            return n.split('').map(function (x) {
                return (1 - x).toString();
            }).join('');
        },
    },
}
</script>

<style>
.instruction {
    margin-top: 4em;
}

.instruction {
    padding-bottom: 2em;
}

h1.title {
    font-size: 4em; 
}

ul {
    padding-inline-start: 0px;
}

ul li {
    list-style: none;
    display: inline;
}

h5 a {
    font-size: 1em;
}

h5 a:hover {
    font-size: 1.2em;
}

h5 ul li {
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
    color: rgb(216, 7, 73);
}

a {
    color: #fff;
    font-size: 1.6em;
    transition: .4s ease-in-out;
}

a:hover {
    font-size: 1.8em;
    color:#dc3545;
    text-decoration: none;
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
    font-size: 1.4rem;
    display: block;
}

p.result {
    margin-top: .6em;
}

span.result {
    font-size: 2rem;
    display: block;
}

@media (max-width: 480px) {
    .instruction {
        margin-top: 2em;
    }

    .list-bits li {
        margin-right: 0;
        display: block;
    }

    .list-bits li:first-child {
        margin-left: 0;
    }

    .list-bits li:not(:last-child):after {
        content: "";
        margin-left: 0;
    }

}

</style>