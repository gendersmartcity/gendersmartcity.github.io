<template>
  <Landing> </Landing>
  <GSC></GSC>
  <Credits></Credits>
</template>

<script>
import Landing from "./components/Landing.vue";
import GSC from "./components/GSC.vue";
import Credits from "./components/Credits.vue";

export default {
  name: "App",
  components: {
    GSC,
    Landing,
    Credits,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Lora:wght@400;500&family=Montserrat:wght@300;400&display=swap");
* {
  font-family: "Montserrat", sans-serif;
}
@import "./assets/css/main.css";
@import "./assets/css/mobile.css";
:root {
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
