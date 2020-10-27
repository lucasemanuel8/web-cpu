<template>
  <div class="container" id="app">
    <Instruction />
  </div>
</template>

<script>
import "bootstrap/dist/css/bootstrap.min.css";
import Instruction from "./components/Instruction.vue";

export default {
  name: "app",
  components: {
    Instruction,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Exo+2:wght@300;400;600;700&display=swap");
body {
  background: #ed213a;
  background: url(./assets/background.png), #eeeeee;
}

input {
  border: none;
  background-image: none;
  box-shadow: none;
}

#app {
  font-family: "Exo 2", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #373a40;
}
</style>
