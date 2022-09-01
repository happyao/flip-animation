<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  <div>
    <App4></App4>
  </div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import App4 from './App4.vue'
export default {
  name: 'App',
  components: {
    //HelloWorld
    App4
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
