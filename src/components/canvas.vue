<template>
  <div>
    <canvas id="mainCanv" width="500" height="500"></canvas>
   <div>Wynik: {{snakeWidth-1}}</div>
  <div v-if="gameover"> Game over</div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
export default Vue.extend({
  name: "CanvasApp",
  data() {
    return {
      kierunek: 2,
      snakeCoords: [{ x: 0, y: 0,r:0 }],
      snakeWidth: 1,
      rand1: 0,
      rand2: 0,
      gameover:false,
      apple_image: new Image(),
      SnakeHead_image: new Image(),
      SnakeBody_image: new Image(),
      Dirt_image: new Image(),
      SnakeTail_image: new Image(),
      SnakeLeft_image: new Image(),
      SnakeRight_image: new Image(),
    };
  },

  methods: {
    createbackground(ctx:CanvasRenderingContext2D) {
      ctx.strokeStyle = "rgba(0,0,0,0.3)";
      this.Dirt_image.src = 'img/Dirt.png';
      for (let i = 0; i <= 500; i += 20) {
        for (let j = 0; j <= 500; j += 20) {
          
           ctx.drawImage(this.Dirt_image, i, j);
           //ctx.strokeRect(i, j, 20, 20);
        }
      }
    },
    init() {
      console.log("init");
            this.SnakeHead_image.src = 'img/SnakeHead.png';
      this.SnakeBody_image.src = 'img/Snake_Body.png';
      this.SnakeTail_image.src='img/Snake_Tail.png';
      this.SnakeLeft_image.src='img/left-body.png';
      this.SnakeRight_image.src='img/right-body.png';
          this.apple_image.src = 'img/Apple.png';

      this.keyEventHandler();
      var canvas:HTMLCanvasElement = document.getElementById("mainCanv") as
                 HTMLCanvasElement;

      var ctx:CanvasRenderingContext2D = canvas.getContext("2d") as CanvasRenderingContext2D;
      this.createbackground(ctx);

      ctx.fillStyle = "green";
      this.move(ctx);
      
      this.drawberry(ctx);
    },
    drawberry(ctx:CanvasRenderingContext2D) {
     
    
      this.rand1 = Math.floor(Math.random() * 25);
      this.rand2 = Math.floor(Math.random() * 25);
        for(let i =0;this.snakeCoords.length>i;i++){
         if (
          this.rand1 * 20== this.snakeCoords[i].x &&
         this.rand2 * 20== this.snakeCoords[i].y
        ){
          this.drawberry(ctx)
        }
      }

      ctx.fillStyle = "red";
      
      //ctx.fillRect(this.rand1 * 20, this.rand2 * 20, 20, 20);
      ctx.drawImage(this.apple_image, this.rand1 * 20, this.rand2 * 20);
    },
    drawImageRot(img:HTMLImageElement,x:number,y:number,width:number,height:number,deg:number,ctx:CanvasRenderingContext2D){
    // Store the current context state (i.e. rotation, translation etc..)
    ctx.save()

    //Convert degrees to radian 
    var rad = deg * Math.PI / 180;

    //Set the origin to the center of the image
    ctx.translate(x + width / 2, y + height / 2);

    //Rotate the canvas around the origin
    ctx.rotate(rad);

    //draw the image    
    ctx.drawImage(img,width / 2 * (-1),height / 2 * (-1),width,height);

    // Restore canvas state as saved from above
    ctx.restore();
},
    drawsnake(ctx:CanvasRenderingContext2D) {



      ctx.fillStyle = "green";
      for (let i = 0; i <= this.snakeWidth - 1; i++) {
        
        //ctx.drawImage(this.SnakeHead_image, this.snakeCoords[i].x, this.snakeCoords[i].y); 
        let stopnie=0
        if(this.kierunek==2){
          this.snakeCoords[0].r = 180
        } else if(this.kierunek==1){
          this.snakeCoords[0].r =  0}
          else if(this.kierunek==3){
          this.snakeCoords[0].r = 270}
          else if(this.kierunek==4){
          this.snakeCoords[0].r =  90}
      if(i==0){
        
        this.drawImageRot(this.SnakeHead_image,this.snakeCoords[i].x, this.snakeCoords[i].y,20,20,this.snakeCoords[i].r,ctx);
        
        }else if(i==this.snakeWidth-1){
         this.drawImageRot(this.SnakeTail_image,this.snakeCoords[i].x, this.snakeCoords[i].y,20,20,this.snakeCoords[i].r,ctx)
        
        }
        else{
                if(this.snakeWidth>1&&this.snakeWidth-1!=i){
                  if(this.snakeCoords[i].r!=this.snakeCoords[i+1].r){
                    let r =0
                     if(this.snakeCoords[i+1].r==180){
                      r = 270
                     } else if(this.snakeCoords[i+1].r==0){
                      r =  180}
                      else if(this.snakeCoords[i+1].r==270){
                      r = 0}
                       else if(this.snakeCoords[i+1].r==90){
                      r =  90}

                     if(this.snakeCoords[i].r!>this.snakeCoords[i+1].r){
                        this.drawImageRot(this.SnakeLeft_image,this.snakeCoords[i].x, this.snakeCoords[i].y,20,20,r,ctx)
                    }else{
                         this.drawImageRot(this.SnakeRight_image,this.snakeCoords[i].x, this.snakeCoords[i].y,20,20,r,ctx)

}
                  }else{
                   this.drawImageRot(this.SnakeBody_image,this.snakeCoords[i].x, this.snakeCoords[i].y,20,20,this.snakeCoords[i].r,ctx)

        }
        
        }
        }

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
    move(ctx:CanvasRenderingContext2D) {
      var inter = setInterval(() => {
        ctx.clearRect(0, 0, 500, 500);
        this.createbackground(ctx);
        ctx.fillStyle = "red";
        ctx.drawImage(this.apple_image, this.rand1 * 20, this.rand2 * 20);
        //ctx.fillRect(this.rand1 * 20, this.rand2 * 20, 20, 20);
        this.drawsnake(ctx);
        for (let i = this.snakeWidth - 1; i >= 1; i--) {
          this.snakeCoords[i].x = this.snakeCoords[i - 1].x;
          this.snakeCoords[i].y = this.snakeCoords[i - 1].y;
          this.snakeCoords[i].r = this.snakeCoords[i - 1].r;
          //console.log("2:" + i);
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
      for(let i =1;this.snakeCoords.length>i;i++){
         if (
          this.snakeCoords[0].x == this.snakeCoords[i].x &&
          this.snakeCoords[0].y == this.snakeCoords[i].y
        ){
          this.gameover=true;
        }
      }
      
      if(this.snakeCoords[0].x<0||this.snakeCoords[0].x>=500||this.snakeCoords[0].y<0||this.snakeCoords[0].y>=500){
         this.gameover=true;
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
            r: this.snakeCoords[this.snakeCoords.length - 1].r,
          });
        }
        if(this.gameover==true){
          clearInterval(inter)
        }
      }, 140);
    },
    keyEventHandler() {
      document.addEventListener(
        "keydown",
        (event) => {
          var name = event.key;
          var code = event.code;
          // Alert the key name and key code on keydown
          console.log(`Key pressed ${name} \r\n Key code value: ${code}`);

          if (code == "ArrowUp" && this.kierunek!=2) {
            this.kierunek = 1;
          } else if (code == "ArrowDown"&& this.kierunek!=1) {
            this.kierunek = 2;
          } else if (code == "ArrowLeft"&& this.kierunek!=4) {
            this.kierunek = 3;
          } else if (code == "ArrowRight"&& this.kierunek!=3) {
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
  width: auto;
  height: 80vh;
}
</style>
