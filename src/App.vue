<template>
  <div id="app">
    <ColourChase/>
  </div>
</template>

<script>
import ColourChase from './components/ColourChase'
import HelloWorld from './components/HelloWorld'

export default {
  name: 'App',
  components: {
    ColourChase,
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
