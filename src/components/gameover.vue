<template>
  <div v-if="gameover" id="gameover">
    <div>Game Over</div>
    <div>Wynik: {{ score }}</div>
    <button @click="refresh">Zagraj ponownie</button>
  </div>
</template>
<script lang="ts">
import Vue from "vue";
export default Vue.extend({
  props: ["gameover", "score"],
  name: "CanvasApp",
  methods: {
    refresh() {
      window.location.reload();
    },
  },
});
</script>
<style scoped lang="scss">
#gameover {
  background: rgba(54, 54, 54, 0.8);
  display: block;
  width: 20vw;
  height: 20vh;
  left: 40vw;
  top: 30vh;
  line-height: 5vh;
  font-size: 3vh;
  position: absolute;
  border-radius: 20px;
  color: RED;
  button {
    height: 8vh;
    width: 10vw;
    font-size: 3vh;
    border-radius: 20px;
    background: rgba(42, 197, 28, 0.8);
    border: 1px solid black;
  }
}
</style>
