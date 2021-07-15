<template>
  <Sandbox msg="Welcome to Your Vue.js App" />
</template>

<script>
import Sandbox from "./components/Sandbox.vue";

export default {
  name: "App",
  components: {
    Sandbox,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
*{
  padding:0px;
  margin:0px;
}
</style>
