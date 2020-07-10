<template>

  <div id="app">
     <Header />
    <Svgcommand />
  </div>
</template>

<script>
import Header from './components/layout/Header.vue'
import Svgcommand from './components/Svgcommand.vue'

export default {
  name: 'App',
  components: {
   Header,
   Svgcommand
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
}
</style>
