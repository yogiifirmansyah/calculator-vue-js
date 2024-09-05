<script>
import Calculator from "./components/Calculator.vue";

export default {
  components: {
    Calculator,
  },
};
</script>

<template>
  <div id="app">
    <Calculator />
  </div>
</template>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
