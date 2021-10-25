<template>
  <div id="app">
	<img alt="openness logo" src="./assets/openness-logo.png">
    <HW />
  </div>
</template>

<script>
import HW from './components/HW.vue'

export default {
  name: 'App',
  components: {
    HW
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
