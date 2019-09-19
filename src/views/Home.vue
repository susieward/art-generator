<template>
  <div class="home">
    <div class="bg-colors-container">
      <ul class="bg-colors-ul">
        <li v-for="color in bgColors">
          {{ color }}
        </li>
      </ul>
    </div>
    <CanvasComponent></CanvasComponent>
  </div>
</template>

<script>
export default {
  name: 'home',
  components: {
    CanvasComponent: () => import('@/components/CanvasComponent.vue')
  },

  computed: {

    bgColors(){

    }
  }
}
</script>
<style>
.home {
  display: grid;
  width: 100%;
  grid-template-columns: 1fr auto 1fr;
  justify-content: center;
}

</style>
