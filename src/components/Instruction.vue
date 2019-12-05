<template>
    <div class="instruction">
        <h1>Instruction</h1>
        <form>
            <div class="form-group">
                <label for="registrador_ax">Registrador AX:</label>
                <input v-model="reg_ax" type="text">
            </div>
            <div class="form-group">
                <label for="registrador_bx">Registrador BX:</label>
                <input v-model="reg_bx" type="text">
            </div>
            <button v-on:click="and" type="button">METHOD</button>
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
        };
    },
    mounted() {
    },
    methods: {
        xor() { 
            this.result = (this.reg_ax ^ this.reg_bx) ? 1 : 0; 
        },
        and() { 
            this.result = (this.reg_ax & this.reg_bx) ? 1 : 0;  
        },
        nand() { 
            this.result = !(this.reg_ax & this.reg_bx) ? 1 : 0; 
        },
        or() { 
            this.result = (this.reg_ax | this.reg_bx) ? 1 : 0;
        },
    },
}
</script>

<style lang="stylus">

</style>