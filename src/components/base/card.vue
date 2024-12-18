<script setup>
const props = defineProps({
  img: {
    type: String,
    required: true,
  },
  index:{
    type: Number,
    required: true,
  },
  isFliped:{
    type: Boolean,
    required: true,
  },
});
const emits = defineEmits(['click'])
</script>

<template>
  <transition name="flip">
    <!-- <v-card elevation="2" class="rounded-lg d-flex align-center justify-center" :key="isFliped" -->
    <v-card elevation="2" class="rounded-lg d-flex align-center justify-center" :class="isFliped ? '':'card-primary-color'"
      @click="emits('click', !isFliped)" width="80" height="80">
      <template v-if="isFliped">
        <v-img :src="img"/>
      </template>
      <template v-else>
        <p>{{ index }}</p>
      </template>
    </v-card>
  </transition>
</template>


<style scoped>
.flip-enter-active,
.flip-leave-active {
  transition: transform 0.5s ease;
}

.flip-enter {
  transform: rotateY(180deg);
}

.flip-leave-to {
  transform: rotateY(180deg);
}

.flip-leave,
.flip-enter-to {
  transform: rotateY(0deg);
}
</style>

