<template>
  <div class="timer-widget">
    <img src="@/assets/stopwatch-low-opt.svg" alt="Timer">
    <p id="time">30</p>
  </div>
</template>

<script>
export default {
  name: "Timer",
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

.timer-widget {
  display: flex;
  flex-flow: row nowrap;
  justify-content: center;
}
</style>
