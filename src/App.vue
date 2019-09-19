<template>
  <div id="app">
    <div id="nav">

        <span class="title">a portrait of the artist as a young bot</span>

    </div>
    <router-view/>
  </div>
</template>

<style lang="scss">
#app {
  display: grid;
  min-height: 100vh;
  min-width: 100vw;
  justify-content: center;
  align-content: flex-start;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #222;
}
#nav {
  padding: 30px 0;
  a {
    font-weight: bold;
    color: #2c3e50;
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}

.title {
  font-family: 'Fira Mono Regular';
  font-weight: 500;
}
</style>
