<template>
  <label class="mt-4" :for="idSlider">
    {{ labelName }}
  </label>
  <div class="d-flex">
    <input
        type="range"
        class="form-range"
        :id="idSlider"
        :min="minRange"
        :max="maxRange"
        :value="inputValue"
        @input="$emit('changeValue', $event.target.value, idSlider)"
        @change="$emit('setValue')"
    >
    <span class="font-weight-bold ml-2">{{ inputValue }}</span>
  </div>
</template>

<script>
export default {
  props: {
    minRange: Number,
    maxRange: Number,
    idSlider: String,
    labelName: String,
    inputValue: Number,
  },
  emits: ['changeValue', 'setValue'],
}
</script>

<style scoped>
  .form-range {
    padding-right: 1rem;
  }
</style>