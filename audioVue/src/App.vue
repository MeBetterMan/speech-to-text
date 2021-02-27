<template>
  <div id="app">
    <router-view/>
    <img id="logo" src="./assets/fri_logo.png" height="200" width="200" />
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0px;
}
#logo {
  text-align: center;
  color: #2c3e50;
  margin-top: -5%;
  margin-right: -88%;
  width: 4%;
  height:6%;
}
</style>
