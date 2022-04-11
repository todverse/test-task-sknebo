<template>
  <div id="app">
    <MapYa />
  </div>
</template>

<script>
import MapYa from './components/MapYa.vue'

export default {
  name: 'App',
  components: {
    MapYa,
  }
}
</script>

<style>
#app {
  height: 500px;
}
.ymap-container {
  height: 100%;
}
</style>
