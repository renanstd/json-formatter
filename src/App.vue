<template>
  <div id="app">
    <h1>JSON Formatter</h1>
    <Formatter/>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}
</style>

<script>
import Formatter from '@/components/Formatter.vue'

export default {
  name: 'App',
  components: {
    Formatter
  }
}
</script>
