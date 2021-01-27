<template>
  <!-- <Landing> </Landing> -->
  <GSC></GSC>
</template>

<script>
// import Landing from "./components/Landing.vue";
import GSC from "./components/GSC.vue";

export default {
  name: "App",
  components: {
    GSC,
    // Landing,
  },
};

</script>

<style>
:root{
  --animation-time: 3s;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color: #000000;
  color: #2c3e50;
  /* margin-top: 60px; */
  margin: none;
  padding: none;
  scroll-snap-type: y proximity;
  scroll-padding-top: 40px;
}
body {
  background-color: #000000;
  margin: 0;
}

#app div {
  scroll-snap-align: center;
} 
</style>
