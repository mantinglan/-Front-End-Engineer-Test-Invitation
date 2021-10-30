<template>
  <div id="app">
    <!-- <img src="./assets/logo.png"> -->
    <Banner/>
    <Content></Content>
  </div>
</template>

<script>
import Banner from './components/Banner'
import Content from './components/Content'

export default {
  name: 'App',
  components: {
    Banner,
    Content
  }
}
</script>

<style>
#app {

  text-align: center;
  color: #2c3e50;
  width: 100%;
  min-height: 100vh;
}
</style>
