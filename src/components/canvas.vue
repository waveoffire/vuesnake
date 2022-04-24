<template>
  <div>
    <canvas id="mainCanv" width="500" height="500"></canvas>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
export default Vue.extend({
  name: "CanvasApp",
  data() {
    return {
      kierunek: 2,
      snakeX: 0,
      snakeY: 0,
      snakeWidth: 3,
    };
  },

  methods: {
    createbackground(ctx) {
      ctx.strokeStyle = "rgba(0,0,0,0.3)";

      for (let i = 0; i <= 500; i += 20) {
        for (let j = 0; j <= 500; j += 20) {
          ctx.strokeRect(i, j, 20, 20);
        }
      }
    },
    init() {
      console.log("init");
      this.keyEventHandler();
      var canvas = document.getElementById("mainCanv");

      var ctx = canvas.getContext("2d");
      this.createbackground(ctx);

      ctx.fillStyle = "green";
      this.move(ctx);
    },
    move(ctx) {
      var inter = setInterval(() => {
        ctx.clearRect(0, 0, 500, 500);
        this.createbackground(ctx);
        for (let i = 0; i <= this.snakeWidth; i++) {
          ctx.fillRect(this.snakeX, this.snakeY - 20 * i, 20, 20);
        }
        if (this.kierunek == 1) {
          this.snakeY -= 20;
        } else if (this.kierunek == 2) {
          this.snakeY += 20;
        } else if (this.kierunek == 3) {
          this.snakeX -= 20;
        } else if (this.kierunek == 4) {
          this.snakeX += 20;
        }
      }, 200);
    },
    keyEventHandler() {
      document.addEventListener(
        "keydown",
        (event) => {
          var name = event.key;
          var code = event.code;
          // Alert the key name and key code on keydown
          console.log(`Key pressed ${name} \r\n Key code value: ${code}`);

          if (code == "ArrowUp") {
            this.kierunek = 1;
          } else if (code == "ArrowDown") {
            this.kierunek = 2;
          } else if (code == "ArrowLeft") {
            this.kierunek = 3;
          } else if (code == "ArrowRight") {
            this.kierunek = 4;
          }
        },
        false
      );
    },
  },
  mounted() {
    this.init();
  },
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
#mainCanv {
  border: 1px solid black;
}
</style>
