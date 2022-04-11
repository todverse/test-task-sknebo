<template>
  <div id="app">
    <Loading />
    <MapYa />
  </div>
</template>

<script>
import MapYa from './components/MapYa'
import Loading from './components/Loading'

export default {
  name: 'App',
  components: {
    MapYa,
    Loading,
  }
}
window.onload = () => {
  document.getElementById('loader').style.display = 'none';
};
</script>

<style>
#app {
  height: 500px;
}
.ymap-container {
  height: 100%;
}
</style>
