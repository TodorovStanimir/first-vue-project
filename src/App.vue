<template>
  <div id="app">
    <Cards />
  </div>
</template>

<script>
import Cards from "./components/Cards.vue";

export default {
  name: "App",
  components: {
    Cards
  }
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body {
  height: 100vh;
  display: flex;
  background-image: linear-gradient(120deg, #d4fc79 0%, #96e6a1 100%);
}

#app {
  width: 100%;
  padding: 7%;
}
</style>
