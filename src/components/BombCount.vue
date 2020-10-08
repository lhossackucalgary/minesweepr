<template>
  <div class="flags-widget">
    <img src="@/assets/flag-opt.svg" alt="Flag">
    <p>40</p>
  </div>
</template>

<script>
export default {
  name: 'BombCount',
  components: {

  },
  data () {
    return {

    }
  }
}
</script>

<style scoped>
img {
  height: 1.5em;
}

.flags-widget {
    display: flex;
    flex-flow: row nowrap;
    justify-content: center;
}
</style>
