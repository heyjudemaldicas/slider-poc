<template>
  <!-- <SliderComponent /> -->
  <VueformSlider />
</template>

<script>
import VueformSlider from './components/VueformSlider.vue';
//import SliderComponent from './components/Slider.vue';

export default {
  name: 'App',
  components: {
    //SliderComponent,
    VueformSlider,
  }
}
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
