<template>
  <h1 class="font-serif text-6xl">Simon Says</h1>
  <Principal></Principal>
</template>

<script>
import Principal from './components/Principal.vue'

export default {
  name: 'App',
  components: {
    Principal
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
h1{
  font-family: Avenir, Arial, Helvetica, sans-serif;
  font-style: oblique;

}
</style>
