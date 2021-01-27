<template>
  <div class="landing">
    <transition name="head">
      <h1 class="landing-text">Gendering the Smart City</h1>
    </transition>
  </div>
</template>

<script>
export default {};
</script>

    
<style>
h1 {
  font-size: 6em;
}
.landing {
  height: 100vh;
  display: flex;
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
</style>
