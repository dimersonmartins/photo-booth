<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/interface.svg" width="50px">
    <h4>Photo booth</h4><br>
    <Photobooth/>
  </div>
</template>

<script>
import Photobooth from '@/components/Photobooth'

export default {
  name: 'App',
  components: {
    Photobooth
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
