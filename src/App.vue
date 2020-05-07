<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" width="50px">
    <h4>Example of how to use webcam with Vuejs, Electron and Boostrap</h4><br>
    <MyWebCam/>
  </div>
</template>

<script>
import MyWebCam from '@/components/MyWebCam'

export default {
  name: 'App',
  components: {
    MyWebCam
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
