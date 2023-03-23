<template>
  <div id="app">
    <JsPlumb />
  </div>
</template>

<script>
import JsPlumb from './components/JsPlumb.vue'
export default {
  name: 'App',
  components: {
    JsPlumb
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
