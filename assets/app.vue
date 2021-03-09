<template>
  <div id="app">
    <pixi />
  </div>
</template>

<script>
import pixi from "./components/PixiView.vue";

export default {
  name: "App",
  components: {
    pixi
  }
};
</script>

<style>
body,
html {
  position: relative;
  margin: 0;
  padding: 0;
  height: 100%;
  width: 100%;
  overflow: hidden;
}
</style>