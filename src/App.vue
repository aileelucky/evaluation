<template>
  <div id="app">
    <Evaluation msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import Evaluation from './components/evaluation/Evaluation.vue'

export default {
  name: 'App',
  components: {
    Evaluation
  }
}
</script>

<style>
body{
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100%;
  width: 100%;
  position:fixed;
}
</style>
