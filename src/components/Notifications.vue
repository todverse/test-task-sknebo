<template>
  <div id='notification'>
    {{ msg }}
  </div>
</template>

<script>
  export default {
    name: 'Notification',
    props: {
      msg: String,
    }
  }
</script>

<style scoped>
  div {
    width: 300px;
    height: 100px;
    background-color: rgba(255,255,255,0.8);
    border-radius: 10px;
    text-align: center;
    padding-top: 20px;
    margin: 10px;
    position: absolute;
    right: 0;
    z-index: 999999;
    display: none;
  }
</style>