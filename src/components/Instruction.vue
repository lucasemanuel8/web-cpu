<template>
    <div class="instruction">
        <h1>Instruction</h1>
        <form>
            <div class="form-group">
                <label for="instruction">Instrução:</label>
                <input v-model="instruction" type="text">
            </div>
            <div class="form-group">
                <label for="registrador_ax">Registrador AX:</label>
                <input v-model="reg_ax" type="text">
            </div>
            <div class="form-group">
                <label for="registrador_bx">Registrador BX:</label>
                <input v-model="reg_bx" type="text">
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