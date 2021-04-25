<template>
  <app-card>
    <template v-slot:headerCard>
      Основные настройки
    </template>
    <template v-slot:contentCard>
      <div class="row mt-4">
        <div class="col">
          <button
              type="button"
              class="btn btn-primary btn-block"
              :disabled="data.isActive"
              @click="changeData(1, 'isActive', mode); $emit('setData', mode)"
          ><i class="fas fa-lightbulb"></i>
          </button>
        </div>
        <div class="col">
          <button type="button"
                  class="btn btn-primary btn-block"
                  :disabled="!data.isActive"
                  @click="changeData(0, 'isActive', mode); $emit('setData', mode)"
          ><i class="far fa-lightbulb"></i>
          </button>
        </div>
      </div>
      <div class="row mt-4">
        <div class="col-lg">
          <button
              type="button"
              class="btn btn-primary btn-block"
              @click="$emit('saveData')"
          >
            Сохранить
          </button>
        </div>
      </div>
      <app-input-slider
          :min-range="0"
          :max-range="100"
          :input-value="data.brightness"
          id-slider="brightness"
          label-name="Яркость"
          @changeValue="changeData"
          @setValue="$emit('setData', mode)"
      ></app-input-slider>
    </template>
  </app-card>
</template>

<script>
import AppCard from "./AppCard"
import AppInputSlider from "./AppInputSlider"

export default {
  components: {
    AppCard,
    AppInputSlider,
  },
  props: {
    data: Object,
    mode: String,
  },
  methods: {
    changeData(value, id) {
      let dataOut = this.data
      dataOut[id] = parseInt(value)
      this.$emit('changeData', dataOut, this.mode)
    }
  }
}
</script>

<style scoped>

</style>