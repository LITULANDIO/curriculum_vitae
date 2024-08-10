<template>
  <Particles
    id="tsparticles"
    :options="options"
    :particlesInit="particlesInit"
  />
</template>

<script setup lang="ts">
import { ref, onMounted, nextTick } from 'vue';
import  { Engine } from "tsparticles-engine";
import { loadSlim } from "tsparticles-slim"; 
import jsIcon from '../assets/js.png';
import htmlIcon from '../assets/html5.png';
import cssIcon from '../assets/css.jpg';
import vueIcon from '../assets/vue.png';
import jsonIcon from '../assets/json.png';
import storybookIcon from '../assets/storybook.png';
import tailwindcssIcon from '../assets/tailwindcss.png';
import figmaIcon from '../assets/figma.png';
import vscIcon from '../assets/visual_studio_code.png'
import nuxtIcon from '../assets/nuxt.png'
import nodeIcon from '../assets/nodejs.png'
import gitIcon from '../assets/git.png' 

const options = ref()
onMounted(() => {
  nextTick(() => {
    options.value = {
        background: {
          color: "#000",
        },
        fullScreen: {
          enable: true,
          zIndex: -1,
        },
        interactivity: {
          events: {
            onClick: {
              enable: true,
              mode: "push",
            },
            onHover: {
              enable: true,
              mode: "repulse",
            },
          },
          modes: {
            push: {
              quantity: 10,
            },
            repulse: {
              distance: 300,
            },
          },
        },
        particles: {
          shape: {
          type: "image",
          image: [
            { src: jsIcon, width: 60, height: 60 },
            { src: htmlIcon, width: 60, height: 60 },
            { src: cssIcon, width: 60, height: 60 },
            { src: vueIcon, width: 60, height: 60 },
            { src: jsonIcon, width: 60, height: 60 },
            { src: storybookIcon, width: 60, height: 60 },
            { src: tailwindcssIcon, width: 60, height: 60 },
            { src: figmaIcon, width: 60, height: 60 },
            { src: vscIcon, width: 60, height: 60 },
            { src: nuxtIcon, width: 60, height: 60 },
            { src: nodeIcon, width: 60, height: 60 },
            { src: gitIcon, width: 60, height: 60 },
          ],
        },
          links: {
            distance: 200,
            enable: true,
          },
          move: {
            enable: true,
            speed: { min: 1, max: 5 },
          },
          number: {
            value: 100,
          },
          opacity: {
            value: { min: 0.3, max: 0.7 },
          },
          size: {
            value: 20
          },
        }
      }

})
  })

    
    const particlesInit = async (engine: Engine) => {
      await loadSlim(engine);
    }  
</script>

<style>
#tsparticles {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: #000;
  z-index: 9;
  overflow: hidden;
}
</style>