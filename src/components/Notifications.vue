<template>
  <div id='notification'>
    <section v-for='elem in arr' :key='elem.msg'>
      <p>{{ elem.msg }}</p>
      <p>{{ elem.dist }}</p>
      <p>{{ elem.distAir }}</p>
    </section>
  </div>
</template>

<script>
  export default {
    name: 'Notification',
    props: {
      arr: Array,
    }
  }
</script>

<style scoped>
  div {
    width: 300px;
    position: absolute;
    margin: 10px;
    right: 0;
    z-index: 999999;
  }
  section {
    width: 300px;
    min-height: 100px;
    padding-top: 20px;
    padding-bottom: 20px;
    margin-top: 10px;
    background-color: rgba(255,255,255,0.9);
    border-radius: 10px;
    text-align: center;
  }
</style>