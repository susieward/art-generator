<template>
  <div id="app">
    <div id="nav">
      <div>
        <span class="title">canvas art generator</span>
      </div>
      <div style="display: grid; align-content: flex-start; justify-content: flex-end; grid-auto-columns: auto; grid-auto-flow: column; grid-column-gap: 60px">

      <!--  <div style="position: relative" @mouseenter="showDropdown = true" @mouseleave="showDropdown = false">
          <span style="display: block; margin-bottom: 20px; cursor: default">canvas size</span>
          <div v-if="showDropdown" style="top: 25; left: 0; position: absolute; background-color: #222; width: 500px">
            <ul>
              <li @click="selectOption(option.value)" v-for="(option, i) in sizeOptions" :key="i">
                {{ option.text }}
              </li>
              <li>
                height (px): <input type="text" v-model="customHeight" /> width (px): <input type="text" v-model="customWidth" />
              </li>
            </ul>
          </div>
        </div> -->

        <a :href="url" @click="downloadCanvas" download="canvas.png">
          download image
        </a>
          <span style="cursor: pointer" @click="generateCanvas">
            generate new canvas
          </span>
      </div>
    </div>
    <div class="content">
      <CanvasComponent :height="height" :width="width" ref="canvasComp" @bgColor="setBgHex" />
    </div>
  </div>
</template>
<script>
import CanvasComponent from '@/components/CanvasComponent.vue'
export default {
  data(){
    return {
      url: '',
      currentBgHex: '',
      height: window.innerHeight,
      width: window.innerWidth,
      selectedOption: null,
      showDropdown: false,
      sizeOptions: [
        {
          text: 'screen size (default)',
          value: { height: window.innerHeight, width: window.innerWidth }
        },
        {
          text: '700 x 900',
          value: { height: 700, width: 900 }
        },
      ]
    }
  },
  name: 'App',
  components: {
    CanvasComponent
  },
  methods: {
    selectOption(val){
      this.selectedOption = val
      this.height = val.height;
      this.width = val.width
      this.generateCanvas()
    },
    downloadCanvas(){
      let canvas = this.$refs.canvasComp.$refs.canvas;
      var dataURL = canvas.toDataURL('image/png');
      this.url = dataURL;
    },
    setBgHex(hex){
      this.currentBgHex = hex
    },
    generateCanvas(){
      let canvas = this.$refs.canvasComp
      canvas.generateCanvas()
    }
  }
}
</script>
<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  padding: 0;
  background-color: #444;
}

a {
  color: #fff;
  text-decoration: none;
}


#app {
  display: grid;
  min-height: 100vh;
  width: 100vw;
  align-content: flex-start;
  padding: 20px;
  margin: 0;
  font-family: 'Fira Mono Regular';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #fff;
  position: relative;

}
#nav {
  display: grid;
  grid-template-columns: auto auto;
    position: relative;
    z-index: 10;
}

.title {
  font-family: 'Fira Mono Regular';
  font-weight: 500;
  color: #fff;
}

.content {
  display: grid;
  padding: 0;
  margin: 0;
  min-width: 100%;
  height: auto;
}

.home {
  display: grid;
  max-width: 100%;
  height: auto;
  grid-template-columns: auto auto;
  justify-content: flex-start;
  grid-column-gap: 15px;
}

.sidebar {
  display: grid;
  align-content: flex-start;
  justify-content: flex-start;
  grid-auto-rows: auto;
}
</style>
