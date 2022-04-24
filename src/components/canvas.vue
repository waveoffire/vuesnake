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
      snakeCoords: [{ x: 0, y: 0 }],
      snakeWidth: 1,
      rand1: 0,
      rand2: 0,
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
      this.drawberry(ctx);
    },
    drawberry(ctx) {
      this.rand1 = Math.floor(Math.random() * 25);
      this.rand2 = Math.floor(Math.random() * 25);
      ctx.fillStyle = "red";
      ctx.fillRect(this.rand1 * 20, this.rand2 * 20, 20, 20);
    },
    drawsnake(ctx) {
      ctx.fillStyle = "green";
      for (let i = 0; i <= this.snakeWidth - 1; i++) {
        ctx.fillRect(this.snakeCoords[i].x, this.snakeCoords[i].y, 20, 20);
      }

      /*  for (let i = 0; i <= this.snakeWidth - 1; i++) {
        if (this.snakeCoords.length <= this.snakeWidth - 1) {
          this.snakeCoords.push({ x: 0, y: 0 });
          console.log("1:" + i);
        }
      }
      for (let i = 0; i <= this.snakeWidth - 1; i++) {
        if (i > 0) {
          this.snakeCoords[i].x = this.snakeCoords[i - 1].x;
          this.snakeCoords[i].y = this.snakeCoords[i - 1].y;
          console.log("2:" + i);
        }
      }
      for (let i = 0; i <= this.snakeWidth - 1; i++) {
        ctx.fillRect(this.snakeCoords[i].x, this.snakeCoords[i].y, 20, 20);
        console.log("3:" + i);
      } */
    },
    move(ctx) {
      var inter = setInterval(() => {
        ctx.clearRect(0, 0, 500, 500);
        this.createbackground(ctx);
        ctx.fillStyle = "red";
        ctx.fillRect(this.rand1 * 20, this.rand2 * 20, 20, 20);
        this.drawsnake(ctx);
        for (let i = 1; i <= this.snakeWidth - 1; i++) {
          this.snakeCoords[i].x = this.snakeCoords[i - 1].x;
          this.snakeCoords[i].y = this.snakeCoords[i - 1].y;
          console.log("2:" + i);
        }

        if (this.kierunek == 1) {
          this.snakeCoords[0].y -= 20;
        } else if (this.kierunek == 2) {
          this.snakeCoords[0].y += 20;
        } else if (this.kierunek == 3) {
          this.snakeCoords[0].x -= 20;
        } else if (this.kierunek == 4) {
          this.snakeCoords[0].x += 20;
        }

        if (
          this.snakeCoords[0].x == this.rand1 * 20 &&
          this.snakeCoords[0].y == this.rand2 * 20
        ) {
          this.snakeWidth++;
          this.drawberry(ctx);
          this.snakeCoords.push({
            x: this.snakeCoords[this.snakeCoords.length - 1].x,
            y: this.snakeCoords[this.snakeCoords.length - 1].y,
          });
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
