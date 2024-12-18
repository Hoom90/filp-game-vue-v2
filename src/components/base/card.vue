<script setup>
import {ref } from "vue"
const props = defineProps({
  img: {
    type: String,
    required: true,
  },
  index:{
    type: String,
    required: true,
  },
  isFliped:{
    type: Boolean,
    required: true,
  }
});
const emits = defineEmits(['click'])
</script>

<template>
  <transition name="flip">
    <v-card elevation="2" class="rounded-lg" @click="emits('click',isFliped)">
      <p v-if="isFliped">{{ index }}</p>
      <v-img v-else :src="img" aspect-ratio="1" class="rounded-lg"></v-img>
    </v-card>
  </transition>
</template>
