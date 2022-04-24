<template>
  <div id="app">
    <canvas-app />
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import Canvas from "./components/canvas.vue";
export default Vue.extend({
  components: {
    CanvasApp: Canvas,
  },
  name: "APP",
  data() {
    return {};
  },
  methods: {},
});
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
