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
@import url('https://fonts.googleapis.com/css?family=Lato|Lilita+One&display=swap');
body {
  background: #360b2f;  /* fallback for old browsers */
  background: url("https://www.transparenttextures.com/patterns/cubes.png"), -webkit-linear-gradient(to left, #c31432, #360b2f); /* Chrome 10-25, Safari 5.1-6 */
  background: url("https://www.transparenttextures.com/patterns/cubes.png"), linear-gradient(to left, #c31432, #360b2f); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}

input {
  border:none;
  background-image:none;
  box-shadow: none;
}

#app {
  font-family: 'Lilita One', cursive;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
}

</style>
