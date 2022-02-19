<template>
  <div id="app">
    <b-navbar toggleable class="p-4 bg-dark">
      <b-navbar-brand href="#">
        <p class="h2 bold text-orange">
          Create custom motivation letter using GPT-3!
        </p>
      </b-navbar-brand>
    </b-navbar>
    <Main/>
  </div>
</template>

<script>
import Main from './components/Main.vue'

export default {
  name: 'App',
  components: {
    Main
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

.bold{
  font-weight: bold !important;
}

.bg-dark{
  background-color: #264653;
}
</style>
