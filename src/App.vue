<script setup>
import { ref } from "vue"
import baseCard from "@/components/base/card.vue"
const assetsLength = 8
const totalMatch = assetsLength
const defaultCounter = 1
const defaultTime = 120
const matchCounter = ref(0)
const counter = ref(defaultCounter)
const time = ref(defaultTime)
const timerStarted =ref(false)
const message = ref(null)

let timerInterval = null

const flipedCards = ref([])
const images = ref(
  [...getAssets(), ...getAssets()]
    .sort(() => Math.random() - 0.5)
    .map((image, index) => ({
      src: image,
      id: index + 1,
      isFliped: false,
    }))
)

const startOver =()=>{
  matchCounter.value = 0
  counter.value = defaultCounter
  time.value = defaultTime
  message.value = null
  images.value = [...getAssets(), ...getAssets()]
    .sort(() => Math.random() - 0.5)
    .map((image, index) => ({
      src: image,
      id: index + 1,
      isFliped: false,
    }))
}

function getAssets() {
  return Array.from({ length: assetsLength }, (_, index) => `/src/assets/images/product-${index + 1}.jpg`)
}

const toggleCard = (image) => {
  if(!timerStarted.value){
    startTimer()
  }

  if(flipedCards.value.length != 2 && !image.isFliped && counter.value != 0 && time.value != 0){
    image.isFliped = true
    counter.value--
  
    flipedCards.value.push(image)
    if (flipedCards.value.length == 2) compareCards()
    if(counter.value == 0){
      console.error('Counter is up!');
      message.value = 'ای وای!!! حرکت های شما تمام شد...'
      clearInterval(timerInterval)
    }
  }

  if(matchCounter.value == totalMatch){
    clearInterval(timerInterval)
    console.log('Hooray')
    message.value = 'تبریک!!! شما برنده شدید'
  }
}

const compareCards = () =>{
  if (flipedCards.value.at(0).src !== flipedCards.value.at(1).src) {
    setTimeout(() => {
      flipedCards.value.at(0).isFliped = false
      flipedCards.value.at(1).isFliped = false
      flipedCards.value = []
    }, 1000)
  }else{
    matchCounter.value++
    flipedCards.value = []
  }
}

const startTimer = () => {
  if (!timerStarted.value) {
    timerStarted.value = true
    timerInterval = setInterval(() => {
      if (time.value > 0) {
        time.value--
      } else {
        clearInterval(timerInterval)
        console.error("Time is up!");
        message.value = 'آخ آخ آخ!!! زمان شما تمام شد...'
      }
    }, 1000);
  }
};
</script>

<template>
  <v-app>
    <v-main>
      <v-container class="d-flex flex-column justify-center align-center h-100">
        <v-card flat variant="text" class="d-flex justify-space-between pa-1" width="400">
          <p>
            <strong>{{ `زمان: ${Math.floor(time / 60)}:${time - Math.floor(time / 60) * 60}` }}</strong>
          </p>
          <p>
            <strong>{{ `تعداد حرکت: ${counter}` }}</strong>
          </p>
        </v-card>
        <v-card flat variant="text" class="d-flex justify-center align-center ga-3 flex-wrap pa-1" width="400">
          <baseCard v-for="(image, index) in images " :key="index" :img="image.src" :index="index + 1"
            :isFliped="image.isFliped" @click="toggleCard(image)" />
        </v-card>
      </v-container>
      <v-overlay v-model="message" class="d-flex justify-center align-center" :persistent="true">
        <v-card width="300" class="text-center">
          <v-card-text>
            <p class="mb-5">
              <strong>{{ message }}</strong>
            </p>
            <v-btn color="warning" @click="startOver">دوباره</v-btn>
          </v-card-text>
        </v-card>
      </v-overlay>
    </v-main>
  </v-app>
</template>

<style>
@font-face{
  font-family: "vazir";
  src : url('@/assets/font/Vazirmatn-Regular.ttf');
}
*{
  font-family: vazir;
}
.card-primary-color {
  background-color: #DA498D;
  color: #fff;
}
</style>
