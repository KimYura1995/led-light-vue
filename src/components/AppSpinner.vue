<template>
  <transition name="fade">
    <div v-if="isActive" class="text-center">
      <div class="spinner-border" style="width: 3rem; height: 3rem" role="status">
        <span class="visually-hidden">Загрузка...</span>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    isActive: {
      type: Boolean,
      required: true,
    }
  }
}
</script>

<style scoped>
  .spinner-border {
    position: fixed;
    z-index: 998;
    margin: auto;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
  }

  .fade-enter-active,
  .fade-leave-active {
    transition: opacity .5s;
  }

  .fade-enter-from,
  .fade-leave-to {
    opacity: 0;
  }
</style>