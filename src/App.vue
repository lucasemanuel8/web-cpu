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

  background: #ED213A;  /* fallback for old browsers */
  background: url(./assets/background.png), -webkit-linear-gradient(to right, rgb(138, 6, 6), #ED213A);  /* Chrome 10-25, Safari 5.1-6 */
  background: url(./assets/background.png), linear-gradient(to right, rgb(138, 6, 6), #ED213A); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
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
