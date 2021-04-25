<template>
  <app-card>
    <template v-slot:headerCard>
      <div class="row">
        <div class="col">
          <button
              type="button"
              class="btn btn-primary btn-block"
              :disabled="mode === 'static'"
              @click="$emit('changeMode', 'static')"
          >Static
          </button>
        </div>
        <div class="col">
          <button
              type="button"
              class="btn btn-primary btn-block"
              :disabled="mode === 'rainbow'"
              @click="$emit('changeMode', 'rainbow')"
          >Rainbow
          </button>
        </div>
      </div>
      <div class="row mt-4">
        <div class="col">
          <button
              type="button"
              class="btn btn-primary btn-block"
              :disabled="mode === 'loop'"
              @click="$emit('changeMode', 'loop')"
          >Loop
          </button>
        </div>
        <div class="col">
          <button
              type="button"
              class="btn btn-primary btn-block"
              :disabled="mode === 'breath'"
              @click="$emit('changeMode', 'breath')"
          >Breath
          </button>
        </div>
      </div>
    </template>
    <template v-slot:contentCard>
      <transition name="fade" mode="out-in">
        <div v-if="mode === 'static'">
          <app-palette
              :red="data.static.redColor"
              :green="data.static.greenColor"
              :blue="data.static.blueColor"
          ></app-palette>
          <app-input-slider
              :min-range="0"
              :max-range="255"
              id-slider="redColor"
              label-name="R"
              :input-value="data.static.redColor"
              @changeValue="changeData"
              @setValue="$emit('setData', mode)"
          ></app-input-slider>
          <app-input-slider
              :min-range="0"
              :max-range="255"
              id-slider="greenColor"
              label-name="G"
              :input-value="data.static.greenColor"
              @changeValue="changeData"
              @setValue="$emit('setData', mode)"
          ></app-input-slider>
          <app-input-slider
              :min-range="0"
              :max-range="255"
              id-slider="blueColor"
              label-name="B"
              :input-value="data.static.blueColor"
              @changeValue="changeData"
              @setValue="$emit('setData', mode)"
          ></app-input-slider>
        </div>
        <div v-else-if="mode === 'rainbow'">
          <app-input-slider
              :min-range="1"
              :max-range="5"
              id-slider="rainbowStep"
              label-name="Шаг"
              :input-value="data.rainbow.rainbowStep"
              @changeValue="changeData"
              @setValue="$emit('setData', mode)"
          ></app-input-slider>
        </div>
        <div v-else-if="mode === 'loop'">
          <app-input-slider
              :min-range="1"
              :max-range="5"
              id-slider="loopStep"
              label-name="Шаг"
              :input-value="data.loop.loopStep"
              @changeValue="changeData"
              @setValue="$emit('setData', mode)"
          ></app-input-slider>
        </div>
        <div v-else-if="mode === 'breath'">
          <app-palette
              :red="data.breath.redColorBreath"
              :green="data.breath.greenColorBreath"
              :blue="data.breath.blueColorBreath"
          ></app-palette>
          <app-input-slider
              :min-range="0"
              :max-range="100"
              id-slider="minBright"
              label-name="Минимальная яркость"
              :input-value="data.breath.minBright"
              @changeValue="changeData"
              @setValue="$emit('setData', mode)"
          ></app-input-slider>
          <app-input-slider
              :min-range="0"
              :max-range="100"
              id-slider="maxBright"
              label-name="Максимальная яркость"
              :input-value="data.breath.maxBright"
              @changeValue="changeData"
              @setValue="$emit('setData', mode)"
          ></app-input-slider>
          <app-input-slider
              :min-range="1"
              :max-range="5"
              id-slider="breathStep"
              label-name="Шаг"
              :input-value="data.breath.breathStep"
              @changeValue="changeData"
              @setValue="$emit('setData', mode)"
          ></app-input-slider>
          <app-input-slider
              :min-range="0"
              :max-range="255"
              id-slider="redColorBreath"
              label-name="R"
              :input-value="data.breath.redColorBreath"
              @changeValue="changeData"
              @setValue="$emit('setData', mode)"
          ></app-input-slider>
          <app-input-slider
              :min-range="0"
              :max-range="255"
              id-slider="greenColorBreath"
              label-name="G"
              :input-value="data.breath.greenColorBreath"
              @changeValue="changeData"
              @setValue="$emit('setData', mode)"
          ></app-input-slider>
          <app-input-slider
              :min-range="0"
              :max-range="255"
              id-slider="blueColorBreath"
              label-name="B"
              :input-value="data.breath.blueColorBreath"
              @changeValue="changeData"
              @setValue="$emit('setData', mode)"
          ></app-input-slider>
        </div>
      </transition>
    </template>
  </app-card>
</template>

<script>
import AppCard from './AppCard'
import AppInputSlider from "./AppInputSlider"
import AppPalette from "./AppPalette"
export default {
  components: {
    AppCard,
    AppInputSlider,
    AppPalette,
  },
  props: {
    mode: String,
    data: Object,
  },
  methods: {
    changeData(value, id) {
      let dataOut = this.data[this.mode]
      dataOut[id] = parseInt(value)
      this.$emit('changeData', dataOut, this.mode)
    }
  }
}
</script>

<style scoped>
  .card {
    margin-top: 2rem;
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