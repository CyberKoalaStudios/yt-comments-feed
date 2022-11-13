<template>
<!--  <img alt="Vue logo" src="./assets/logo.png">-->
  <FetchComments/>
</template>

<script>
import FetchComments from "./components/FetchComments.vue";

export default {
  name: 'App',
  components: {
    FetchComments
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, Lato, Inter,  sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /*margin-top: 60px;*/
}
</style>
