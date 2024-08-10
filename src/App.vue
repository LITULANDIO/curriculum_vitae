<script setup>
import Terminal from './components/Terminal.vue'
import Particles from './components/Particles.vue'
</script>

<template>
   <div class="app-container">
      <Particles />
      <Terminal />
  </div>
</template>

<style>
html, body {
  margin: 0;
  padding: 0;
  height: 100%;
  overflow-x: hidden;
}

.app-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #000;
  overflow: hidden;
  position: relative;
}

#particles-js {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.terminal-container {
  position: relative;
  z-index: 10;
}
</style>
