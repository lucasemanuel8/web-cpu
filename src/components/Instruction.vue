<template>
    <div class="instruction">
        <h1>Instruction</h1>
        <a href="#" v-on:click="setBits(4)" class="select-bits" id="4-bits">4 bits</a>
        <a href="#" v-on:click="setBits(8)" class="select-bits" id="8-bits">8 bits</a>
        <a href="#" v-on:click="setBits(16)" class="select-bits" id="16-bits">16 bits</a>
        <form>
            <div class="form-group">
                <label for="instruction">Instrução:</label>
                <input v-model="instruction" type="text">
            </div>
            <div class="form-group">
                <label for="registrador_ax">Registrador AX:</label>
                <input v-model="reg_ax" type="text" class="reg" maxlength="4">
            </div>
            <div class="form-group">
                <label for="registrador_bx">Registrador BX:</label>
                <input v-model="reg_bx" type="text" class="reg" maxlength="4">
            </div>
            <button v-on:click="select_instruction" type="button" class="btn btn-info">START</button>
        </form>
        <p>Resultado: {{ result }}</p>
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
            if (n.length < this.bits) 
                return this.setZerosInLeft(n);     
            return n;
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
input {
    background-color: rgba(255, 255, 255, .2);
    border-radius: .4em;
    padding: 0 .4em;
    color: #fff;
}
</style>