<script setup>
import baseCard from "@/components/base/card.vue";

const images = Array.from({ length: 8 }, (_, index) =>
  {
    return {
      src:`/src/assets/images/product-${index + 1}.jpg`,
      isFliped:false,
    }
  }
)
</script>

<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row>
          <v-col
            cols="3"
            v-for="(image, index) in images"
            :key="index"
          >
            <base-card :img="image.src" :index="index+1" :isFliped="image.isFliped" @click="image.isFliped = !image.isFliped"/>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>
