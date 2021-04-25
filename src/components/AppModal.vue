<template>
  <transition name="fade">
    <div
        v-if="isShow"
        class="modal"
    >
      <div class="modal-dialog">
        <div
            class="modal-content"
            tabindex="0"
            @blur="$emit('closeModal')"
        >
          <div class="modal-body">
            <slot></slot>
          </div>
          <div class="modal-footer">
            <button
                type="button"
                class="btn btn-primary"
                @click="$emit('closeModal')"
                >
              <i class="fas fa-times"></i>
            </button>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    isShow: {
      type: Boolean,
      required: true,
    }
  },
  emits: ['closeModal'],
}
</script>

<style scoped>

.modal {
  display: block;
}

.modal:before {
  content: '';
  background: #000;
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  opacity: 0.4;
}

.modal-dialog {
  margin: 8rem auto;
  opacity: 1;
}

</style>