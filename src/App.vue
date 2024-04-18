<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <ClientForm />
  </div>
</template>

<script>
import ClientForm from './components/ClientForm.vue';

export default {
  name: 'App',
  components: {
    ClientForm
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
