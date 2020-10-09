<template>
  <div class="">
    <select name="difficulty" id="difficulty">
      <option value="Easy">Easy</option>
      <option value="Medium">Medium</option>
    </select>
  </div>
</template>

<script>
export default {
  name: 'Difficulty',
  components: {

  },
  data () {
    return {

    }
  }
}
</script>

<style scoped>
select {
  padding: 0.25em;
  border-radius: 0.5em;
  background-color: white;
}
</style>
