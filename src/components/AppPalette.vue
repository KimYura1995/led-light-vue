<template>
  <div
      class="palette mt-4 w-100 rounded shadow"
      :style="dynamicColor"
  >
  </div>
</template>

<script>
export default {
  props: {
    red: Number,
    blue: Number,
    green: Number,
  },
  computed: {
    dynamicColor() {
      return {
        'background-color': `rgb(${this.red},${this.green},${this.blue})`
      }
    }
  },
}
</script>

<style scoped>
  .palette {
    height: 3rem;
    width: 3rem;
  }
</style>