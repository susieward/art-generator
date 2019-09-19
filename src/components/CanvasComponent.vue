<template>
  <div>
<canvas @click="generateCanvas" id="canvas" ref="canvas" width="800" height="550"></canvas>
<p>
  {{ bgColors }}
</p>
<p>
  {{ currentBgColor }}
</p>
<p>
  <button @click="saveBgColor">save background color</button>
</p>
<p>
  <button @click="generateCanvas">generate new canvas</button>
</p>
</div>
</template>
<script>
export default {
  data(){
    return {
    currentColor: null,
    currentBgHex: null,
    currentBgColor: null,
    bgColors: [],
    lineColors: [],
    shades: []
    }
  },
  name: 'CanvasComponent',

  mounted: function(){
    this.generateCanvas();

  },

  computed: {

  },

  methods: {
    generateCanvas: function(){
      let c = this.$refs.canvas;
      let ctx = c.getContext("2d");

      this.fillBgColor(c, ctx);

      let randomNum = Math.floor((Math.random() * 1) + 50);
      let interval = setInterval(() => this.draw(c, ctx), randomNum);

      setTimeout(() => {
        clearInterval(interval);
      }, 1000);
    },

    saveBgColor: function(){
      if(this.currentBgHex !== null){
        this.bgColors.push(this.currentBgHex);
      }
    },

    draw: function(c, ctx){

      let canvasWidth = c.width;
      let canvasHeight = c.height;
      let randomX = Math.random()*canvasWidth;
      let randomY = Math.random()*canvasHeight;

      let random1 = Math.floor((Math.random() * 500) + 1);
      let random2 = Math.floor((Math.random() * 4) + 1);
      let randomLineWidth = ((Math.random() * 3));
      let randomHsl = 'hsl('+ 360*Math.random() +',100%,50%)';

      let randomShade = this.shades[Math.floor(Math.random()*this.shades.length)];
      let points = [];
console.log(randomLineWidth)
      ctx.beginPath();
      ctx.shadowBlur = 10;
 ctx.shadowColor = 'rgb(0, 0, 0)';
      ctx.strokeStyle = randomHsl;
		  ctx.lineWidth = randomLineWidth;
		  ctx.moveTo(canvasWidth/2, random1);
		  ctx.lineTo(randomX,randomY);
		  ctx.stroke();
    },

    getMainColor: function(r, g, b){
      let max;
      if(r > g && r > b){
        max = r
      }
      if(g > r && g > b){
        max = g
      }

      if(b > r && b > g){
        max = b;
      }

      if(r === g && r > b){
        max = r;
      }

      if(g === b && g > r){
        max = g;
      }

      return max;
    },

    fillBgColor: function(c, ctx){
      let randomRgb = this.getRandomRgb();

      let r = randomRgb.r;
      let g = randomRgb.g;
      let b = randomRgb.b;

      let bgColor = this.buildRandomColor(randomRgb);
      let hex = this.rgbToHex(r, g, b);

    let baseColor = this.getMainColor(r, g, b);


    let vals = [r, g, b];
    let shade = this.getShades(vals);
    let shade2 = this.getShades(shade);
    let shade3 = this.getShades(shade2);
    let shade4 = this.getShades(shade3);
    let shade5 = this.getShades(shade4);
    let shade6 = this.getShades(shade5);
    let shade7 = this.getShades(shade6);

    let shades = [shade, shade2, shade3, shade4, shade5, shade6, shade7];

    let randomShade = shades[Math.floor(Math.random()*shades.length)];
    this.shades = shades;
    //  console.log(shades);

      console.log('bgColor', bgColor);

      this.currentBgColor = bgColor;
      this.currentBgHex = hex;
      ctx.fillStyle = bgColor;
      ctx.fillRect(0, 0, c.width, c.height);
    },

    getShades: function(vals){

        return vals.map(v => v + 10);
    },


    getRandomRgb: function(){
      let r = Math.round(Math.random()*0xFF);
      let g = Math.round(Math.random()*0xFF)
      let b = Math.round(Math.random()*0xFF);

      return { r: r, g: g, b: b }
    },

    buildRandomColor: function(randomRgb){

      let color = "rgb("+[
        randomRgb.r,
        randomRgb.g,
        randomRgb.b
        ].join()+")";
      return color
    },

  generateHslaColors: function(saturation, lightness, amount) {
    let colors = []
    let huedelta = Math.trunc(360 / amount)

    for (let i = 0; i < amount; i++) {
      let hue = i * huedelta
      colors.push(`hsl(${hue},${saturation}%,${lightness}%)`)
    }

  return colors
},

    getRandomColor: function(){

      let randomColor = this.getRandomRgb();
      let color = "rgb("+[
          randomColor.r,
          randomColor.g,
          randomColor.b
        ].join()+")";

      return color
    },

    componentToHex: function(c) {
      let hex = c.toString(16);
      return hex.length == 1 ? "0" + hex : hex;
    },

    rgbToHex: function(r, g, b) {
      return "#" + this.componentToHex(r) + this.componentToHex(g) + this.componentToHex(b);
    },
    }
  }

</script>
<style>
</style>
