<template>
  <app-header></app-header>
  <main class="container content">
    <card-option
        :data="data.main"
        mode="main"
        @changeData="changeData"
        @setData="sendData"
        @saveData="saveData"
    ></card-option>
    <mode-pills
        :mode="currentMode"
        :data="data"
        @changeMode="changeMode"
        @setData="sendData"
    ></mode-pills>
    <app-spinner :isActive="isActiveSpinner"></app-spinner>
    <app-modal
        :isShow="isShowModal"
        @closeModal="closeModal"
    >
      {{ modalText }}
    </app-modal>
  </main>
</template>

<script>
import axios from 'axios'

import AppHeader from './components/AppHeader'
import AppSpinner from "./components/AppSpinner"
import AppModal from "./components/AppModal"
import CardOption from "./components/CardOption"
import ModePills from "./components/ModePills"

const ERROR_CODE = 'ECONNABORTED'
const ERROR_MESSAGE = 'Не удалось подключиться к устройству'
const DATA_MAP_IN = {
  'led_status': 'isActive',
  'led_brightness': 'brightness',
  'red_color': 'redColor',
  'green_color': 'greenColor',
  'blue_color': 'blueColor',
  'rainbow_step': 'rainbowStep',
  'loop_step': 'loopStep',
  'min_brightness': 'minBright',
  'max_brightness': 'maxBright',
  'breath_step': 'breathStep',
  'red_color_breath': 'redColorBreath',
  'blue_color_breath': 'blueColorBreath',
  'green_color_breath': 'greenColorBreath',
}

const DATA_MAP_OUT = {
  'isActive': 'led_status',
  'brightness': 'led_brightness',
  'redColor': 'red_range',
  'blueColor': 'blue_range',
  'greenColor': 'green_range',
  'rainbowStep': 'rainbow_step',
  'loopStep': 'loop_step',
  'minBright': 'min_brightness_range',
  'maxBright': 'max_brightness_range',
  'breathStep': 'breath_step',
  'redColorBreath': 'breath_red_range',
  'greenColorBreath': 'breath_green_range',
  'blueColorBreath': 'breath_blue_range',
}

const DATA_MODE = {
  'main': 'main_settings',
  'static': 'static_mode',
  'rainbow': 'rainbow_mode',
  'loop': 'color_loop_mode',
  'breath': 'breath_mode',
}


export default {
  name: 'App',
  data() {
    return {
      url: 'http://192.168.0.146/',
      modalText: '',
      isShowModal: false,
      isActiveSpinner: false,
      currentMode: 'main',
      data: {
        main: {
          isActive: 0,
          brightness: 0,
        },
        static: {
          redColor: 0,
          greenColor: 0,
          blueColor: 0,
        },
        rainbow: {
          rainbowStep: 0,
        },
        loop: {
          loopStep: 0,
        },
        breath: {
          minBright: 0,
          maxBright: 0,
          breathStep: 0,
          redColorBreath: 0,
          blueColorBreath: 0,
          greenColorBreath: 0,
        }
      },
    }
  },
  components: {
    AppHeader,
    AppSpinner,
    AppModal,
    CardOption,
    ModePills,
  },
  created() {
    this.loadData()
  },
  methods: {
    async loadData() {
      await this.sendRequest({'main_settings': 'get'}, 'main')
    },
    async changeData(data, mode) {
      for (let key in data) {
        this.data[mode][key] = data[key]
      }
    },
    parseData(data, mode) {
      for (let key in data) {
        this.data[mode][DATA_MAP_IN[key]] = data[key]
      }
    },
    parseErrors(error) {
      if (error.code === ERROR_CODE) {
        this.showModal(ERROR_MESSAGE)
      }
    },
    async sendRequest(params, mode = '') {
      this.showSpinner()
      await axios
          .get(this.url, {
            params: params,
            timeout: 5000,
          })
          .then(response => {
            if (mode) {
              this.parseData(response.data, mode)
            }
            this.closeSpinner()
          })
          .catch(err => {
            this.parseErrors(err)
            this.closeSpinner()
          })
    },
    async sendData(mode) {
      let dataOut = {}
      for (let key in this.data[mode]) {
        dataOut[DATA_MAP_OUT[key]] = this.data[mode][key]
      }
      await this.sendRequest(dataOut)
    },
    async saveData() {
      await this.sendRequest({'save': 'save'})
    },
    showSpinner() {
      this.isActiveSpinner = true
    },
    closeSpinner() {
      this.isActiveSpinner = false
    },
    showModal(text = '') {
      this.isShowModal = true
      this.modalText = text
    },
    closeModal() {
      this.isShowModal = false
    },
    async changeMode(mode) {
      this.currentMode = mode
      let outputParam = {}
      outputParam[DATA_MODE[this.currentMode]] = 'get'
      await this.sendRequest(outputParam,  this.currentMode)
    }
  }
}
</script>

<style>
  .content {
    margin-top: 3rem;
    padding:0 20rem;
    padding-bottom: 3rem;
  }
  
  @media (max-width: 1199px) {
    .content {
      padding: 0 0;
    }
  }

</style>
