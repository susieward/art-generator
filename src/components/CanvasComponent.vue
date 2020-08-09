<template>
  <div>
<canvas @click="generateCanvas" id="canvas" ref="canvas" width="800" height="550"></canvas>
<p>
<span v-for="color in bgColors">
  <a :href="colorUrl(color)" target="_blank">{{ color }}</a>,
</span>
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

    colorUrl: function(color){
      let c = color.substr(1);
      return `https://www.color-hex.com/color/${c}`;

    },

    generateCanvas: function(){
      const c = this.$refs.canvas;
      const ctx = c.getContext("2d");

      this.fillBgColor(c, ctx);

      let randomNum = Math.floor((Math.random() * 1) + 50);
      let interval = setInterval(() => this.drawRandomRects(c, ctx), randomNum);

      setTimeout(() => {
        clearInterval(interval);
      }, 2000);
    },

    saveBgColor: function(){
      if(this.currentBgHex !== null){
        this.bgColors.push(this.currentBgHex);
      }
    },

    drawRandomRects: function(c, ctx){

      const canvasWidth = c.width;
      const canvasHeight = c.height;
      let randomX = Math.random()*canvasWidth;
      let randomY = Math.random()*canvasHeight;

      let random1 = Math.floor((Math.random() * 500) + 1);
      let random2 = Math.floor((Math.random() * 4) + 1);
      let randomLineWidth = ((Math.random() * 4));
      let randomHsl = 'hsl('+ 360*Math.random() +',100%,50%)';

    for(let i=0; i < 1000; i++) {
      ctx.beginPath();
      let x = Math.random() * canvasWidth;
      let y = Math.random() * canvasHeight;
      let width = Math.random() * canvasWidth;
      let height = Math.random() * canvasHeight;
      ctx.strokeStyle = randomHsl;
      ctx.rect(x, y, width, height);
    }
    ctx.stroke();

/*
    //  let randomShade = this.shades[Math.floor(Math.random()*this.shades.length)];
      let points = [];
      console.log(randomLineWidth)
      ctx.beginPath();
      ctx.moveTo(randomX, randomY);
    //ctx.lineTo(randomX, randomY);
      ctx.bezierCurveTo(290, -40, 200, 200, 400, 100);
      ctx.shadowBlur = 10;
      ctx.shadowColor = 'rgb(0, 0, 0)';
    // ctx.lineJoin = ctx.lineCap = 'round';
      ctx.strokeStyle = randomHsl;
		  ctx.lineWidth = randomLineWidth;
  //  ctx.arc(100, 75, 50, 0, Math.PI/2);
		  ctx.stroke();
*/
    },

    drawNeighborPoints: function(c, ctx){

      const canvasWidth = c.width;
      const canvasHeight = c.height;

      let randomX = Math.random()*canvasWidth;
      let randomY = Math.random()*canvasHeight;
    let randomLineWidth = ((Math.random() * 2));
    let randomHsl = 'hsl('+ 360*Math.random() +',100%,50%)';

      let connectingX = 0;
      let connectingY = 0;

      for(let i=0; i < 1000; i++) {

        let randomStartX = Math.random()*canvasWidth;
        let randomStartY = Math.random()*canvasHeight;

        let controlX = randomStartX - Math.random()*canvasWidth;
        let controlY = randomStartY - Math.random()*canvasHeight;


        let endX = Math.random()*canvasWidth;
        let endY =  Math.random()*canvasHeight;

        ctx.beginPath();
        ctx.moveTo(randomStartX, randomStartY);
        ctx.strokeStyle = randomHsl;
        ctx.lineWidth = randomLineWidth;
        ctx.bezierCurveTo((randomStartX * Math.random()), (randomStartY + Math.random()*canvasHeight), controlX, controlY, endX, endY);

  ctx.moveTo(endX, endY);
        ctx.strokeStyle = 'hsl('+ 360*Math.random() +',100%,50%)';
  		  ctx.lineWidth = randomLineWidth;

          ctx.bezierCurveTo((endX - Math.random()), (endY + Math.random()*canvasHeight), (Math.random()*canvasWidth/2), (Math.random()*canvasHeight/2), (Math.random()*canvasWidth), (Math.random()*canvasHeight/2));
// ctx.moveTo(Math.random()/canvasWidth, Math.random()/canvasHeight);
          ctx.strokeStyle = 'hsl('+ 360*Math.random() +',100%,50%)';
    		  ctx.lineWidth = randomLineWidth;
          ctx.bezierCurveTo((endX - Math.random()), (endY + Math.random()*canvasHeight), (Math.random()*canvasWidth/2), (Math.random()*canvasHeight/2), (Math.random()*canvasWidth*2), (Math.random()*canvasHeight/2));

      }
      ctx.shadowBlur = 10;
      ctx.shadowColor = 'rgb(0, 0, 0)';
  ctx.stroke();
//  this.drawRandomRects(c, ctx)
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
