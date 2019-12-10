<template>
  <div class="container" id="app">
    <Instruction/>
  </div>
</template>

<script>
import 'bootstrap/dist/css/bootstrap.min.css';
import Instruction from './components/Instruction.vue';

export default {
  name: 'app',
  components: {
    Instruction
  }
}
</script>

<style>
body {
  background: #0F2027;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to left, #2C5364, #203A43, #0F2027);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to left, #2C5364, #203A43, #0F2027); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}
input {
  border:none;
  background-image:none;
  box-shadow: none;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
}
</style>
