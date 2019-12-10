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
            <button v-on:click="select_instruction" type="button" class="btn btn-info">METHOD</button>
        </form>
        <p>Resultado: {{ result }}</p>
    </div>
</template>

<script>
export default {
    name: 'Instruction',
    data() {
        return {
            reg_ax: 0,
            reg_bx: 0,
            result: 0,
            instruction: '',
            bits: 8,
        };
    },
    mounted() {
    },
    methods: {
        select_instruction() {
            this[this.instruction.toLowerCase()]();
        },
        xor() { 
            this.result = this.convertToBin(this.convertToDec(this.reg_ax) ^ this.convertToDec(this.reg_bx)); 
        },
        and() { 
            this.result = this.convertToBin(this.convertToDec(this.reg_ax) & this.convertToDec(this.reg_bx)); 
        },
        nand() { 
            this.result = this.invertBits(this.convertToBin(this.convertToDec(this.reg_ax) & this.convertToDec(this.reg_bx)));
        },
        or() { 
            this.result = this.convertToBin(this.convertToDec(this.reg_ax) | this.convertToDec(this.reg_bx));
        },
        nor() { 
            this.result = this.invertBits(this.convertToBin(this.convertToDec(this.reg_ax) | this.convertToDec(this.reg_bx)));
        },
        xnor() {
            this.result = this.invertBits(this.convertToBin(this.convertToDec(this.reg_ax) ^ this.convertToDec(this.reg_bx)));
        },
        setBits(bits) {
            this.bits = bits;
            document.querySelectorAll(".reg").forEach(function (input) {
                input.maxLength = bits;
            });
        },
        invertBits(n) {
            return n.split('').map(function (x) {
                return (1 - x).toString();
            }).join('');
        },
        convertToDec(n) {
            return parseInt(n, 2).toString(10);
        },
        convertToBin(n) {
            return (n >>> 0).toString(2);
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