<template>
  <div id="app">
    <h1>Scroll down, then scroll inside the middle container &#8595;</h1>
    <Placeholder v-for="i in 5" :key="i" />
    <ObservedContainer />
    <Placeholder v-for="i in 5" :key="i + 'p2'" />
  </div>
</template>

<script>
import Placeholder from "./components/Placeholder";
import ObservedContainer from "./components/ObservedContainer";

export default {
  name: "App",
  components: {
    Placeholder,
    ObservedContainer,
  },
};
</script>

<style>
body {
  background-color: #f7fafc;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
h1 {
  margin-bottom: 50px;
}
</style>
