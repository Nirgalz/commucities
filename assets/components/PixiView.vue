<template>
  <div class="pixiBody">
    <div class="buttons">
      <button v-on:click="stop()">Stop</button>
      <button v-on:click="start()">Start</button>
      <button v-on:click="reset()">Reset</button>
    </div>
  </div>
</template>

<script>
import * as PIXI from "pixi.js";

let introAnim;
const stars = [];
let t = 0;
export default {
  name: "pixi",
  ready: function() {},
  methods: {
    stop() {
      introAnim.stop();
    },
    start() {
      introAnim.start();
    },
    reset() {
      console.log("reset");
      for (let i = 0; i < stars.length; i++) {
        const star = stars[i];
        star.x = 0;
        star.y = 0;
      }
    },
    bigBang() {}
  },
  created() {
    // 1. Create a Pixi renderer and define size and a background color
    introAnim = new PIXI.Application({
      width: window.innerWidth,
      height: window.innerHeight,
      // change background color to blue
      backgroundColor: "0x000000"
    });

    // 2. Append canvas element to the body
    document.body.appendChild(introAnim.view);

    const wW = window.innerWidth,
        wH = window.innerHeight,
        cw = window.innerWidth / 2,
        ch = window.innerHeight / 2;

    const minX = wW / 4,
        maxX = minX + cw,
        minY = wH / 4,
        maxY = minY + ch;

    console.log(wH, minY, maxY);

    let animSpeed = 0.05;
    const increase = (Math.PI * 2) / 800;
    const c = 0;

    const starNumber = 500;
    let starSize = 2,
        starY = 0,
        starX = 0;
    const starAlpha = 1,
        starScale = 1,
        starSpeed = 1;

    // 3. Create a container that will hold your scene
    const container = new PIXI.Container();

    introAnim.stage.addChild(container);

    let i;
    for (i = 0; i < starNumber; i++) {
      // 4b. Create a circle
      var star = new PIXI.Graphics();

      // define fill of our circle
      star.beginFill(0xffffff, 1);

      starSize = Math.random() * 2 + 0.75;
      star.animSpeed = Math.random() * 1;
      star.aDirection = 0;
      star.cDirection = 0;

      star.tint = Math.random() * 0xffffff;
      star.tintTo = star.tint;
      starX = Math.floor(Math.random() * (maxX - minX + 1)) + minX;
      starY = Math.floor(Math.random() * (maxY - minY + 1)) + minY;

      star.initX = starX;
      star.initY = starY;

      star.dix = starX - cw;
      star.diy = starY - ch;
      star.angle = (Math.atan2(star.diy, star.dix) * 180) / Math.PI;
      star.radians = Math.atan2(star.diy, star.dix);

      // draw circle (x, y, radius)
      star.drawCircle(starX, starY, starSize);

      stars.push(star);

      container.addChild(star);
    }

    // Listen for animate update
    introAnim.ticker.add(function(delta) {
      animSpeed = Math.abs(Math.cos(c)) * 15 + Math.pow(Math.PI, Math.atan(c));

      t++;

      for (let i = 0; i < stars.length; i++) {
        const star = stars[i];

        const b = star.getBounds();

        if (
            b.x > window.innerWidth ||
            b.x < 0 ||
            b.y > window.innerHeight ||
            b.y < 0
        ) {
          star.x = 0;
          star.y = 0;
        } else {
          star.x += Math.cos(star.radians) * star.animSpeed * animSpeed;
          star.y += Math.sin(star.radians) * star.animSpeed * animSpeed;
        }
      }
    });

    /*
    window.addEventListener("resize", function() {
      var wW = window.innerWidth,
        wH = window.innerHeight;

      console.log(wW, wH);

      app.renderer.resize(wW, wH);

      for (var i = 0; i < stars.length; i++) {
        var star = stars[i];
        star.x = Math.floor(Math.random() * wH + 1);
        star.y = Math.floor(Math.random() * wW + 1);
      }
    });
    */
  }
};
</script>

<style lang="scss">
body {
  height: 100%;
}
.buttons {
  position: fixed;
}
</style>