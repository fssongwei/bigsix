<template>
  <div id="app">
    <Wheel />
  </div>
</template>

<script>
import Wheel from "./Wheel.vue";

export default {
  name: "App",
  components: {
    Wheel,
  },
};
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
