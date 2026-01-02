<script setup>
import { RouterLink, RouterView } from 'vue-router'
import { ref, onMounted } from 'vue'
</script>

<template>
  <div id="app">
    <!-- Landing Section -->
    <section v-if="!opened" class="landing">
      <div
        class="min-h-screen w-full flex flex-col justify-start items-center px-4">
        <!-- Video element -->
        <!-- <video autoplay muted playsinline class="absolute top-0 left-0 w-full h-full object-cover">
          <source src="/images/vdo.mp4" type="video/mp4">
        </video> -->

        <!-- Photo background -->
        <div class="absolute inset-0 w-full h-full bg-center bg-fixed bg-no-repeat"
          style="background-image: url('/images/bg-curve.jpg'); background-size: contain; @media (min-width: 768px) { background-size: 100% 100%; }">
        </div>

        <!-- Overlay -->
        <!-- <div class="absolute inset-0 bg-black opacity-30"></div> -->

        <div class="relative z-10 flex flex-col justify-center pt-40 items-center text-center">
          <h1 class="text-2xl md:text-2xl font-moul leading-relaxed lg:text-4xl text-[#7B1F2A] text-center py-2">
            សិរីមង្គលអាពាហ៍ពិពាហ៍
          </h1>
          <p class="text-lg md:text-xl lg:text-2xl mb-8 text-[#7B1F2A] text-center max-w-md">
            The Wedding Day
          </p>
          <img src="/images/mark-png-pink.svg" alt="Ornament" class="w-66 mb-8" />
          <h3 class="text-xl md:text-xl font-moul leading-relaxed lg:text-6xl mb-4 text-[#7B1F2A] text-center">
            សូមគោរពអញ្ជើញ
          </h3>
          <p class="text-lg md:text-xl font-moul lg:text-2xl mb-8 text-white text-center max-w-md">
            {{ questName }}
          </p>
          <button
            class="bg-white text-pink-600 font-nokora px-8 py-3 md:px-10 md:py-4 rounded-full hover:bg-pink-50 hover:scale-105 transition-all duration-300 font-semibold text-lg shadow-lg"
            @click="openInvitation">
            បើកធៀប
          </button>
        </div>
      </div>
      <!-- <button class="text-pink" @click="openInvitation">Click to Open</button> -->
    </section>

    <!-- STORY SPLASH -->
    <transition name="fade">
      <section v-if="opened && step === 1" class="story-screen">
        <div
          class="min-h-screen w-full flex flex-col justify-end items-center px-4">
          <!-- Photo background -->
          <img src="/img6.jpg" class="absolute inset-0 w-full h-full object-cover" />
          <div class="absolute bottom-0 left-0 right-0 z-10">
            <transition name="fade-up">
              <div class="bg-black/50 backdrop-blur-sm p-8 text-center">
                <p class="text-white text-2xl font-moul">Save the date</p>
              </div>
            </transition>
          </div>
        </div>
      </section>
    </transition>

    <!-- Main Invitation -->
    <section v-if="opened && step === 2" class="invitation">
      <div
        class="relative min-h-screen w-full flex flex-col justify-start items-center px-4">
        <!-- Photo background -->
        <div class="absolute inset-0 w-full h-full bg-fixed bg-no-repeat"
          style="background-image: url('/images/bg-curve.jpg'); background-size: contain; @media (min-width: 768px) { background-size: 100% 100%; }">
        </div>

        <!-- Overlay -->
        <!-- <div class="absolute inset-0 bg-black opacity-30"></div> -->
        <!-- <Snowf
          :amount="50"
          :size="5"
          :speed="1.5"
          :wind="0"
          :opacity="0.8"
          :swing="1" color="#fff"
          :resize="true" /> -->

        <div class="relative z-10 flex flex-col justify-center pt-40 items-center text-center">
          <h1 class="text-2xl md:text-2xl font-moul leading-relaxed lg:text-4xl text-[#7B1F2A] text-center py-2">
            សិរីមង្គលអាពាហ៍ពិពាហ៍
          </h1>
          <p class="text-lg md:text-xl lg:text-2xl mb-8 text-[#7B1F2A] text-center max-w-md">
            The Wedding Day
          </p>
          <!-- <img src="/images/mark-png-pink.svg" alt="Ornament" class="w-66 mb-8" /> -->
          <div class="grid grid-cols-2 gap-8 font-moul leading-relaxed mb-4 text-[#7B1F2A] text-center max-w-md">
            <div class="grid grid-rows-2 gap-2">
              <p>លោក សុខ សប្បាយ</p>
              <p>អ្នកស្រី ឡុង ដានី</p>
            </div>
            <div class="grid grid-rows-2 gap-2">
              <p>លោក សេង ពិសាល</p>
              <p>អ្នកស្រី​ នូ រ៉ានី</p>
            </div>
          </div>

          <div
            class="backdrop-blur-xs bg-white/15 rounded-3xl p-8 flex flex-col items-center text-center max-w-3xl shadow-xl">
            <h3 class="text-lg md:text-lg font-moul leading-relaxed lg:text-xl mb-4 text-[#7B1F2A] text-center">
              មានកិត្តិយសសូមគោរពអញ្ជើញ
            </h3>
            <p class="text-[#7B1F2A] font-metal leading-loose mb-4">
              ឯកឧត្តម លោកឧកញ៉ា លោកជំទាវ លោក លោកស្រី អ្នកនាង កញ្ញា អញ្ជើញចូលរួមជាអធិបតី
              និងជាភ្ញៀវកិត្តិយសបើម្បីប្រសិទ្ធិពរជ័យ សិរីសួស្តីជ័យមង្គលក្នុងកម្មវិធីរៀបមង្គលអាពាហ៍ពិពាហ៍
              កូនប្រុស​-​កូនស្រី របស់យើងខ្ញុំ
            </p>
            <div class="grid grid-cols-2 gap-8 font-moul leading-relaxed mb-4 text-[#7B1F2A] text-center max-w-md">

            </div>
            <h2
              class="font-moul leading-relaxed lg:text-5xl mb-4 text-[#7B1F2A] text-center flex items-center justify-center gap-2">
              សន វិសាល
              <img src="/images/logo.png" alt="Logo" class="w-36" />
              យ៉ាន់ ស្រីនោ
            </h2>

            <p class="text-[#7B1F2A] font-metal leading-loose mb-4">
              ដែលនឹងប្រព្រឹត្តទៅនៅថ្ងៃសុក្រ ៤រោច ខែមាឃ ឆ្នាំម្សាញ់ សប្តស័ក ពុទ្ធសករាជ ២៥៦៩ ត្រូវនឹងថ្ងៃទី ០៦ ខែកុម្ភៈ
              ឆ្នាំ ២០២៦
            </p>
            <p class="text-[#7B1F2A] font-metal leading-loose mb-4">
              វេលាម៉ោង ៤: 00 រសៀល ​ស្ថិតនៅគេហដ្ឋានខាងស្រី ភូមិដង្កៀបក្តាម ឃុំពាមបន្ទារ ស្រុកកំពង់ត្របែក ខេត្តព្រៃវែង។
              ដោយមេត្រីភាព!
            </p>

            <div class="mt-6">
              <RouterLink to="/main-event"
                class="bg-pink-600 text-white font-nokora px-6 py-2 md:px-8 md:py-3 rounded-full hover:bg-pink-700 hover:scale-105 transition-all duration-300 font-semibold text-lg shadow-lg">
                ព័ត៌មានលម្អិតពិធីមង្គលការ
              </RouterLink>
            </div>
            <div class="p-6 sm:p-8 md:p-12 bg-transparent">
              <h2
                className="text-lg sm:text-xl text-center whitespace-nowrap font-moul text-[#7B1F2A] animate-slide-down bg-white/10 p-3 sm:p-4 rounded-lg">
                កម្មវិធីមង្គលអាពាហ៍ពិពាហ៍
              </h2>
              <div className="flex items-center justify-center mb-6 sm:mb-8 animate-fade-in-up">
                <!-- <img src="/images/kbaj03.PNG" alt="kbaj_kh" className="w-32 sm:w-32 md:w-50" /> -->
              </div>
              <div class="relative">
                <!-- Vertical Timeline Line -->
                <div class="absolute left-1/2 transform -translate-x-1/2 w-0.5 bg-secondary dark:bg-primary/30 h-full">
                </div>

                <!-- Timeline Events -->
                <div class="grid grid-cols-[auto_1fr] gap-x-4 -gap-y-2 text-left">
                  <div class="flex flex-col items-left gap-1 pt-3 relative">
                    <div class="text-primary text-3xl">
                      <img src="/images/ev1.png" alt="kbaj_kh"
                        className="w-18 sm:w-12 md:w-16 rounded-xl bg-transparent mix-blend-multiply" />
                    </div>
                    <div class="w-0.5 bg-secondary dark:bg-primary/30 h-2"></div>
                  </div>
                  <div class="flex flex-1 flex-col pb-6 pt-2">
                    <p class="text-text-light font-nokora text-[#7B1F2A] text-lg font-semibold leading-normal">
                      ពិធីសែនក្រុងពាលី</p>
                    <p class="text-base text-[#e73a6b] font-moul leading-normal">ម៉ោង ០៦ : ០០ នាទីព្រឹក</p>
                  </div>

                  <!-- Timeline Item 1 -->
                  <div class="flex flex-col items-left gap-1 pt-3 relative">
                    <div class="text-primary text-3xl">
                      <img src="/images/ev2.png" alt="kbaj_kh"
                        className="w-18 sm:w-12 md:w-16 rounded-xl bg-transparent mix-blend-multiply" />
                    </div>
                    <div class="w-0.5 bg-secondary dark:bg-primary/30 h-2"></div>
                  </div>
                  <div class="flex flex-1 flex-col pb-6 pt-2">
                    <p class="text-text-light font-nokora text-[#7B1F2A] text-lg font-semibold leading-normal">
                      ជួបជុំភ្ញៀវកិត្តិយសរៀបចំពិធីហែជំនូន</p>
                    <p class="text-base text-[#e73a6b] font-moul leading-normal">ម៉ោង ០៦ : ៣០ នាទីព្រឹក</p>
                  </div>

                  <!-- Timeline Item 2 -->
                  <div class="flex flex-col items-left gap-1 relative">
                    <div class="w-0.5 bg-secondary dark:bg-primary/30 h-2"></div>
                    <div class="text-primary text-3xl">
                      <img src="/images/ev9.png" alt="kbaj_kh"
                        className="w-18 sm:w-12 md:w-16 rounded-xl bg-transparent mix-blend-multiply" />
                    </div>
                    <div class="w-0.5 bg-secondary dark:bg-primary/30 h-2"></div>
                  </div>
                  <div class="flex flex-1 flex-col pb-6 pt-2">
                    <p class="text-text-light font-nokora text-[#7B1F2A] text-lg font-semibold leading-normal">
                      ពិធីហែជំនូន(កំណត់)ចូលរោងជ័យ </p>
                    <p class="text-s text-[#e73a6b] font-moul leading-normal">ម៉ោង ០៧ : ០០ នាទីព្រឹក</p>
                  </div>

                  <!-- Timeline Item 4 -->
                  <div class="flex flex-col items-left gap-1 relative">
                    <div class="w-0.5 bg-secondary dark:bg-primary/30 h-2"></div>
                    <div class="text-primary text-3xl">
                      <img src="/images/ev3.png" alt="kbaj_kh"
                        className="w-18 sm:w-12 md:w-16 rounded-xl bg-transparent mix-blend-multiply" />
                    </div>
                    <div class="w-0.5 bg-secondary dark:bg-primary/30 h-2"></div>
                  </div>
                  <div class="flex flex-1 flex-col pb-6 pt-2">
                    <p class="text-text-light font-nokora text-[#7B1F2A] text-lg font-semibold leading-normal">
                      ពិធីពិសាស្លាកំណត់ និង អញ្ជើញភ្ញៀវកិត្តិយសពិសាអាហារពេលព្រឹក</p>
                    <p class="text-base text-[#e73a6b] font-moul leading-normal">ម៉ោង ០៧ : ៣០ នាទីព្រឹក</p>
                  </div>

                  <!-- Timeline Item 5 -->
                  <div class="flex flex-col items-center gap-1 pb-3 relative">
                    <div class="w-0.5 bg-secondary dark:bg-primary/30 h-2"></div>
                    <div class="text-primary text-3xl">
                      <img src="/images/ev4.png" alt="kbaj_kh"
                        className="w-18 sm:w-12 md:w-16 rounded-xl bg-transparent mix-blend-multiply" />
                    </div>
                  </div>
                  <div class="flex flex-1 flex-col pb-6 pt-2">
                    <p class="text-text-light font-nokora text-[#7B1F2A] text-lg font-semibold leading-normal">
                      ពិធីបំពាក់ចិញ្ចៀន</p>
                    <p class="text-base text-[#e73a6b] font-moul leading-normal">ម៉ោង ៨ : ៣០ នាទីព្រឹក</p>
                  </div>
                  <!-- Timeline Item 6 -->
                  <div class="flex flex-col items-center gap-1 pb-3 relative">
                    <div class="w-0.5 bg-secondary dark:bg-primary/30 h-2"></div>
                    <div class="text-primary text-3xl">
                      <img src="/images/ev5.png" alt="kbaj_kh"
                        className="w-18 sm:w-12 md:w-16 rounded-xl bg-transparent mix-blend-multiply" />
                    </div>
                  </div>
                  <div class="flex flex-1 flex-col pb-6 pt-2">
                    <p class="text-text-light font-nokora text-[#7B1F2A] text-lg font-semibold leading-normal">
                      ពិធីកាត់សក់បង្កក់សិរី</p>
                    <p class="text-base text-[#e73a6b] font-moul leading-normal">ម៉ោង ៩ : ៣០ នាទីព្រឹក</p>
                  </div>
                  <!-- Timeline Item 7 -->
                  <div class="flex flex-col items-center gap-1 pb-3 relative">
                    <div class="w-0.5 bg-secondary dark:bg-primary/30 h-2"></div>
                    <div class="text-primary text-3xl">
                      <img src="/images/ev6.png" alt="kbaj_kh"
                        className="w-18 sm:w-12 md:w-16 rounded-xl bg-transparent mix-blend-multiply" />
                    </div>
                  </div>
                  <div class="flex flex-1 flex-col pb-6 pt-2">
                    <p class="text-text-light font-nokora text-[#7B1F2A] text-lg font-semibold leading-normal">
                      ពិធីបង្វិលពពិល សំពះផ្ទឹមចងដៃ និងបាចផ្កាស្លាពរជ័យ</p>
                    <p class="text-base text-[#e73a6b] font-moul leading-normal">ម៉ោង ១០ : ៣០ នាទីព្រឹក</p>
                  </div>
                  <!-- Timeline Item 8 -->
                  <div class="flex flex-col items-center gap-1 pb-3 relative">
                    <div class="w-0.5 bg-secondary dark:bg-primary/30 h-2"></div>
                    <div class="text-primary text-3xl">
                      <img src="/images/ev7.png" alt="kbaj_kh"
                        className="w-18 sm:w-12 md:w-16 rounded-xl bg-transparent mix-blend-multiply" />
                    </div>
                  </div>
                  <div class="flex flex-1 flex-col pb-6 pt-2">
                    <p class="text-text-light font-nokora text-[#7B1F2A] text-lg font-semibold leading-normal">
                      អញ្ញើញភ្ញៀវកិត្តិយសពិសាអាហារថ្ងៃត្រង់</p>
                    <p class="text-base text-[#e73a6b] font-moul leading-normal">ម៉ោង ១២ : ០០ ថ្ងៃត្រង់</p>
                  </div>
                  <!-- Timeline Item 8 -->
                  <div class="flex flex-col items-center gap-1 pb-3 relative">
                    <div class="w-0.5 bg-secondary dark:bg-primary/30 h-2"></div>
                    <div class="text-primary text-3xl">
                      <img src="/images/ev8.png" alt="kbaj_kh"
                        className="w-18 sm:w-12 md:w-16 rounded-xl bg-transparent mix-blend-multiply" />
                    </div>
                  </div>
                  <div class="flex flex-1 flex-col pb-6 pt-2">
                    <p class="text-text-light font-nokora text-[#7B1F2A] text-lg font-semibold leading-normal">
                      ទទួលបដិសណ្ឋារកិច្ចភ្ញៀវកិត្តិយសពិសារ ភោជនាអាហារដោយមេត្រីភាព</p>
                    <p class="text-base text-[#e73a6b] font-moul leading-normal">ម៉ោង ០៥ : ០០​ ល្ងាច </p>
                  </div>
                </div>
              </div>
            </div>
            <div class="relative">
              <h2
                class="text-lg sm:text-xl text-center whitespace-nowrap font-moul text-[#7B1F2A] animate-slide-down bg-white/10 p-3 sm:p-4 rounded-lg">
                កម្រងរូបភាព</h2>
              <!-- gallery -->
              <div class="grid grid-cols-12 gap-4">
                <div v-for="(img, index) in images" :key="img.id"
                  :class="['relative overflow-hidden rounded-lg cursor-pointer group', getSpanClass(index)]"
                  @click="selectedImg = img.url">
                  <img :src="img.url" :alt="img.alt"
                    class="w-full h-64 object-cover transition-transform duration-300 group-hover:scale-105" />
                  <div
                    class="absolute inset-0 bg-black/20 opacity-0 group-hover:opacity-100 transition-opacity flex items-center justify-center">
                    <span class="text-white font-medium">View Full</span>
                  </div>
                </div>
              </div>

              <Teleport to="body">
                <transition name="fade">
                  <div v-if="selectedImg"
                    class="fixed inset-0 z-[1000] bg-black/90 backdrop-blur-sm flex items-center justify-center p-4"
                    @click="closeLightbox">
                    <button class="absolute top-6 right-6 text-white text-4xl hover:text-gray-300 transition"
                      @click="closeLightbox">
                      &times;
                    </button>

                    <img :src="selectedImg"
                      class="max-w-full max-h-[90vh] object-contain rounded-lg shadow-2xl transition-all" @click.stop />
                    <p class="absolute bottom-6 text-white/60 text-sm">Click anywhere to close</p>
                  </div>
                </transition>
              </Teleport>
            </div>
          </div>
        </div>
      </div>
    </section>

  </div>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.fade-up-enter-active,
.fade-up-leave-active {
  transition: all 0.5s ease;
}

.fade-up-enter-from,
.fade-up-leave-to {
  opacity: 0;
  transform: translateY(100px) translateX(-100px);
}

/* Prevent body scroll when lightbox is open */
.modal-open {
  overflow: hidden;
}
</style>

<script>
export default {
  data() {
    return {
      opened: false,
      questName: '',
      step: 1,
      audio: null,
      selectedImg: null,
      images: [
        { id: 1, url: '/img1.jpg', alt: 'Image 1' },
        { id: 2, url: '/img2.jpg', alt: 'Image 2' },
        { id: 3, url: '/img3.jpg', alt: 'Image 3' },
        { id: 4, url: '/img4.jpg', alt: 'Image 4' },
        { id: 5, url: '/img5.jpg', alt: 'Image 5' },
        { id: 6, url: '/img6.jpg', alt: 'Image 6' },
        { id: 7, url: '/img7.jpg', alt: 'Image 7' },
        { id: 8, url: '/img8.jpg', alt: 'Image 8' },
        { id: 9, url: '/img9.jpg', alt: 'Image 9' },
      ],
    }
  },
  mounted() {
    const urlParams = new URLSearchParams(window.location.search)
    const nameParam = urlParams.get('name')
    if (nameParam) {
      this.questName = decodeURIComponent(nameParam)
    }
  },
  methods: {
    // Determine span class based on index
    getSpanClass(index) {
      if (index < 3) return 'col-span-4';          // Row 1: 3 images (12/3)
      if (index === 3) return 'col-span-12';       // Row 2: 1 image  (12/1)
      if (index > 3 && index < 6) return 'col-span-6'; // Row 3: 2 images (12/2)
      return 'col-span-4';
    },
    openInvitation() {
      //play music
      this.audio = new Audio('/songs/song.mp3')
      this.audio.play()

      this.opened = true

      // Wait 3 seconds then go to next step
      setTimeout(() => {
        this.step = 2
      }, 3000)
    },
    closeLightbox() {
      this.selectedImg = null
    }
  }
}
</script>
