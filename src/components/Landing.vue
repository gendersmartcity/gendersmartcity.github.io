<template>
  <div class="landing">
    <transition name="head">
      <h1 class="landing-text">Gendering the Smart City</h1>
    </transition>
    <div style="display: grid; grid-template-columns: 1fr 1.8fr 1fr">
      <img src="img/mala.svg" alt="" />
      <div>
        <h4 style="text-decoration: underline">How to use this site:</h4>
        <h5>
          Click on “Toggle Filters” in order to filter excerpts by interviewee,
          topic, or community. Selecting an interviewee will bring up all their
          inputs, while browsing by topic or community will bring up excerpts
          relevant to each. “Remove All Filters” will bring you back to the
          default view.
        </h5>
      </div>
      <img src="img/lakshmi.svg" alt="" />
    </div>
  </div>
</template>

<script>
export default {};
</script>

    
<style>
@import url("https://fonts.googleapis.com/css2?family=Lora:wght@400;500&family=Montserrat:wght@300;400&display=swap");
* {
  font-family: "Montserrat", sans-serif;
}
h1 {
  font-size: 6em;
}
.landing {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
  background-color: #222222;
}

.head-enter-from {
  opacity: 0;
  transform: translateY(-40px);
}

.head-enter-active {
  transition: all 3s linear;
}

.head-enter-to {
  opacity: 1;
  transform: translateY(0);
}

h4,
h5 {
  color: #dddddd;
  width: 50%;
  padding-top: 1em;
  padding-bottom: 0.2em;
  margin: 0 auto;
}
</style>
