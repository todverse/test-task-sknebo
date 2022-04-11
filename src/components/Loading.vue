/* eslint-disable */
<template>
  <div id='loader'>
    <div id='blinker'></div>
    <p>Loading.....</p>
  </div>
</template>

<script>
  export default {
    name: 'Loading',
  }
</script>

<style>
  #loader {
    width: 100%;
    margin: 0;
    padding: 0;
    height: 700px;
    position: absolute;
    font-size: 4em;
    font-weight: 800;
    text-align: center;
  }
  #blinker {
    position: absolute;
    top: 0;
    left: 0;
    width: 30px;
    height: 400px;
    background-color: rgba(255,255,255,0.6);
    animation: blink 2s infinite;
  }
  @keyframes blink {
    from {
      left: 30%;
    }
    to {
      left: 60%;
    }
  }
</style>