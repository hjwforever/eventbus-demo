<template>
  <div id="app">
    <EventBusDemo msg="Hello Vue in CodeSandbox!" />
  </div>
</template>

<script>
import EventBusDemo from "./components/EventBusDemo";

export default {
  name: "App",
  components: {
    EventBusDemo,
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
