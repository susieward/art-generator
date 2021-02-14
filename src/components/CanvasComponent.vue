<template>
<canvas
  id="canvas"
  ref="canvas"
  :height="$props.height"
  :width="$props.width"
  style="position: absolute; top: 0; left: 0; z-index: 0">
</canvas>
  <!--
    <p>
      <span v-for="color in bgColors">
        <a :href="colorUrl(color)" target="_blank">{{ color }}</a>,
      </span>
    </p>
    <p>{{ currentBgColor }}</p>

    <p>
      <button @click="saveBgColor">save background color</button>
    </p>
    <p>
      <button @click="generateCanvas">generate new canvas</button>
    </p> -->
</template>
<script>
export default {
  data(){
    return {
      currentBgHex: null,
      currentBgColor: {
        rgb: null,
        hex: null
      },
      bgColors: [],
      shades: null,
      url: '',
    }
  },
  name: 'CanvasComponent',
  props: ['height', 'width'],
  mounted(){
    this.generateCanvas()
  },
  methods: {
    colorHexUrl(hex){
      return `https://www.color-hex.com/color/${hex.substring(1)}`;
    },
    generateCanvas(){
      const c = this.$refs.canvas;
      const ctx = c.getContext("2d");
      this.fillBgColor(c, ctx);
      this.drawRandom(c, ctx)

/*
      let interval = setInterval(() => this.drawRandom(c, ctx), 200);
      setTimeout(() => {
        clearInterval(interval);
      }, 1000);
      */
    },
    fillBgColor(c, ctx){
      this.shades = null
      let { r, g, b } = this.getRandomRgb()
      let bgColor = this.buildRgbString(r, g, b)
      let hex = this.rgbToHex(r, g, b)
      let shades = this.generateShades(r, g, b)
      this.currentBgColor = {
        rgb: bgColor,
        hex: hex
      }
      this.currentBgHex = hex;
      this.$emit('bgColor', hex)
      ctx.fillStyle = bgColor;
      ctx.fillRect(0, 0, c.width, c.height);
      this.shades = shades
    },
    drawRandom(c, ctx){
      const canvasWidth = c.width;
      const canvasHeight = c.height

      let randomX = () => Math.random()*canvasWidth;
      let randomY = () => Math.random()*canvasHeight;
      let randomLineWidth = () => ((Math.random() * 10));
      let randomHsl = () => 'hsl('+ 360*Math.random() +',100%,50%)';
      let rand = () => Math.round(Math.floor(Math.random()*100))

      ctx.lineJoin = ctx.lineCap = 'round'

      let drawn = 0;
      let maxDrawn = 3;
      let currentX = randomX()
      let currentY = randomY()

      while(drawn !== maxDrawn){
        ctx.beginPath()
        drawn += 1
        ctx.moveTo(currentX, currentY)

        ctx.shadowBlur = randomLineWidth()
        ctx.shadowColor = `${this.getRandomColor()}`
        ctx.shadowOffsetX = randomLineWidth()
        ctx.strokeStyle = randomHsl()
  		  ctx.lineWidth = randomLineWidth()

        let coords1 = [...this.getRandomBezier(canvasWidth, canvasHeight)]

        ctx.bezierCurveTo(...coords1);
        ctx.fillStyle = this.getRandomColor()
        ctx.fillRect(...coords1)
        let [x, y] = coords1.slice(4)
        ctx.moveTo(x, y)
        ctx.shadowBlur = randomLineWidth()
        ctx.shadowColor = `${this.getRandomColor()}`
        ctx.shadowOffsetX = randomLineWidth()
        ctx.shadowOffsetY = randomLineWidth()
        ctx.strokeStyle = randomHsl()
  		  ctx.lineWidth = randomLineWidth()



        let coords2 = [...this.getRandomBezier(canvasWidth, canvasHeight)]
        ctx.bezierCurveTo(...coords2)
        ctx.fillStyle = this.getRandomColor()
        ctx.fillRect(...coords2)
        ctx.stroke()

        let [x2, y2] = coords2.slice(4)
        ctx.beginPath()
        ctx.shadowBlur = randomLineWidth()
        ctx.shadowColor = `${this.getRandomColor()}`
        ctx.shadowOffsetX = randomLineWidth()
        ctx.strokeStyle = randomHsl()
  		  ctx.lineWidth = randomLineWidth()
        ctx.moveTo(x2, y2)

        let coords3 = [...this.getRandomBezier(canvasWidth, canvasHeight)]
        ctx.bezierCurveTo(...coords3)

        let [x3, y3] = coords3.slice(4)
        ctx.shadowBlur = randomLineWidth()
        ctx.shadowColor = `${this.getRandomColor()}`
        ctx.shadowOffsetX = randomLineWidth()
        ctx.strokeStyle = randomHsl()
  		  ctx.lineWidth = randomLineWidth()
        ctx.moveTo(x3, y3)

        let coords4 = [...this.getRandomBezier(canvasWidth, canvasHeight)]
        ctx.bezierCurveTo(...coords4)
        ctx.stroke()

        let [x4, y4] = coords4.slice(4)
        currentX = x4
        currentY = y4
      }
    //  ctx.stroke()
    },
    getRandomBezier(cw, ch){
      let rand = Math.round(Math.floor(Math.random()*100))
      return [
        cw * (rand/100),
        ch * ((20+rand)/100),
        cw * (rand/100),
        ch * (rand/100),
        cw * ((10+rand)/200),
        ch * ((100-rand)/100)
      ]
    },
    generateShades(r, g, b){
      let vals = [r, g, b]
      let shades = [currentShade]
      let numShades = 1
      let maxShades = 6
      let currentShade = vals

      while(numShades !== maxShades){
        numShades += 1
        currentShade = this.incrementShade(currentShade)
        shades.push(currentShade)
      }
      return shades
    },
    incrementShade(vals){
      return vals.map(v => v + 10);
    },
    getRandomColor(){
      let { r, g, b } = this.getRandomRgb()
      return this.buildRgbString(r, g, b)
    },
    getRandomRgb(){
      let r = Math.round(Math.random()*0xFF);
      let g = Math.round(Math.random()*0xFF)
      let b = Math.round(Math.random()*0xFF);
      return { r, g, b }
    },
    buildRgbString(r, g, b){
      return `rgb(${[r, g, b].join()})`
    },
    generateHslaColors(saturation, lightness, amount) {
      let colors = []
      let huedelta = Math.trunc(360 / amount)
      for (let i = 0; i < amount; i++) {
        let hue = i * huedelta
        colors.push(`hsl(${hue},${saturation}%,${lightness}%)`)
      }
      return colors
    },
    rgbToHex(r, g, b) {
      return "#" + this.componentToHex(r) + this.componentToHex(g) + this.componentToHex(b);
    },
    componentToHex(c) {
      let hex = c.toString(16);
      return hex.length == 1 ? "0" + hex : hex;
    },
    getMainColorValue(r, g, b){
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
    saveBgColor(){
      if(this.currentBgHex !== null){
        this.bgColors.push(this.currentBgHex);
      }
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

      for(let i = 0; i < 1000; i++) {
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
  }
}
</script>
<style>
#canvas {

}
.box {
  height: 50px;
  width: 50px;
}
</style>
