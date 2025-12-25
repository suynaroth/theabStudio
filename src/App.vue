<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <div id="app">
    <!-- Landing Section -->
    <section v-if="!opened" class="landing">
      <div
        class="min-h-screen w-full flex flex-col justify-start items-center bg-linear-to-br from-purple-500 via-pink-500 to-red-500 px-4">
        <!-- Video element -->
        <!-- <video autoplay muted playsinline class="absolute top-0 left-0 w-full h-full object-cover">
          <source src="/images/vdo.mp4" type="video/mp4">
        </video> -->

        <!-- Photo background -->
        <img src="/images/landing-bg.jpg" alt="Background" class="absolute inset-0 w-full h-full object-cover" />

        <!-- Overlay -->
        <div class="absolute inset-0 bg-black opacity-30"></div>

        <div class="relative z-10 flex flex-col justify-center pt-40 items-center text-center">
          <h1 class="text-3xl md:text-5xl font-moul leading-relaxed lg:text-6xl mb-4 text-pink-400 text-center">
            សិរីមង្គលអាពាហ៍ពិពាហ៍
          </h1>
          <p class="text-lg md:text-xl lg:text-2xl mb-8 text-white text-center max-w-md">
            The Wedding Day
          </p>
          <img src="/images/mark-png-pink.svg" alt="Ornament" class="w-66 mb-8" />
          <button
            class="bg-white text-pink-600 px-8 py-3 md:px-10 md:py-4 rounded-full hover:bg-pink-50 hover:scale-105 transition-all duration-300 font-semibold text-lg shadow-lg"
            @click="openInvitation">
            Open Invitation
          </button>
        </div>
      </div>
      <!-- <button class="text-pink" @click="openInvitation">Click to Open</button> -->
    </section>

    <!-- Main Invitation -->
    <section v-else class="invitation">
      <div
        class="min-h-screen w-full flex flex-col justify-start items-center bg-linear-to-br from-purple-500 via-pink-500 to-red-500 px-4">
        <!-- Photo background -->
        <img src="/images/bg-gate.jpg" alt="Background" class="absolute inset-0 w-full h-full object-cover" />

        <!-- Overlay -->
        <div class="absolute inset-0 bg-black opacity-30"></div>

        <div class="relative z-10 flex flex-col justify-center pt-40 items-center text-center">
          <h1 class="text-4xl md:text-5xl font-moul leading-relaxed lg:text-6xl mb-4 text-pink-400 text-center">
            សិរីំង្គលអាពាហ៍ពិពាហ៍
          </h1>
          <p class="text-lg md:text-xl lg:text-2xl mb-8 text-white text-center max-w-md">
            The Wedding Day
          </p>
          <!-- <img src="/images/mark-png-pink.svg" alt="Ornament" class="w-66 mb-8" /> -->
          <button
            class="bg-white text-pink-600 px-8 py-3 md:px-10 md:py-4 rounded-full hover:bg-pink-50 hover:scale-105 transition-all duration-300 font-semibold text-lg shadow-lg"
            @click="openInvitation">
            Open Invitation
          </button>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      opened: false
    }
  },
  methods: {
    openInvitation() {
      this.opened = true
    }
  }
}
</script>

