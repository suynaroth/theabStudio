<script setup>
import { RouterLink } from 'vue-router'
import { ref, onMounted } from 'vue'

// State management
const opened = ref(false)
const questName = ref('')
const step = ref(1)
const audio = ref(null)
const selectedImg = ref(null)
const showSaveDate = ref(true)
const showEventDate = ref(false)

// Visibility tracking
const visibleElements = ref({
  mainTitle: false,
  subtitle: false,
  parentsNames: false,
  mainCard: false,
  invitationTitle: false,
  invitationText: false,
  coupleNames: false,
  dateInfo: false,
  locationInfo: false,
  detailButton: false,
  timelineSection: false,
  gallerySection: false
})

const timelineHeaderVisible = ref(false)
const galleryTitleVisible = ref(false)
const visibleItems = ref({})
const visibleGalleryItems = ref({})

// Refs for scroll animations
const timelineRefs = ref([])
const galleryRefs = ref([])

// Data
const images = [
  { id: 1, url: '/p1.jpg', alt: 'Image 1' },
  { id: 2, url: '/p2.jpg', alt: 'Image 2' },
  { id: 3, url: '/p3.jpg', alt: 'Image 3' },
  { id: 4, url: '/p4.jpg', alt: 'Image 4' },
  { id: 5, url: '/p4.jpg', alt: 'Image 5' },
  { id: 6, url: '/p4.jpg', alt: 'Image 6' },
  { id: 7, url: '/p4.jpg', alt: 'Image 7' },
  { id: 8, url: '/p4.jpg', alt: 'Image 8' },
  { id: 9, url: '/p4.jpg', alt: 'Image 9' },
]

const timelineEvents = [
  { icon: "/images/ev1.png", title: "ពិធីសែនក្រុងពាលី", time: "ម៉ោង ០៦ : ០០ នាទីព្រឹក" },
  { icon: "/images/ev2.png", title: "ជួបជុំភ្ញៀវកិត្តិយសរៀបចំពិធីហែជំនូន", time: "ម៉ោង ០៦ : ៣០ នាទីព្រឹក" },
  { icon: "/images/ev9.png", title: "ពិធីហែជំនូន(កំណត់)ចូលរោងជ័យ", time: "ម៉ោង ០៧ : ០០ នាទីព្រឹក" },
  { icon: "/images/ev3.png", title: "ពិធីពិសាស្លាកំណត់ និង អញ្ជើញភ្ញៀវកិត្តិយសពិសាអាហារពេលព្រឹក", time: "ម៉ោង ០៧ : ៣០ នាទីព្រឹក" },
  { icon: "/images/ev4.png", title: "ពិធីបំពាក់ចិញ្ចៀន", time: "ម៉ោង ៨ : ៣០ នាទីព្រឹក" },
  { icon: "/images/ev5.png", title: "ពិធីកាត់សក់បង្កក់សិរី", time: "ម៉ោង ៩ : ៣០ នាទីព្រឹក" },
  { icon: "/images/ev6.png", title: "ពិធីបង្វិលពពិល សំពះផ្ទឹមចងដៃ និងបាចផ្កាស្លាពរជ័យ", time: "ម៉ោង ១០ : ៣០ នាទីព្រឹក" },
  { icon: "/images/ev7.png", title: "អញ្ញើញភ្ញៀវកិត្តិយសពិសាអាហារថ្ងៃត្រង់", time: "ម៉ោង ១២ : ០០ ថ្ងៃត្រង់" },
  { icon: "/images/ev8.png", title: "ទទួលបដិសណ្ឋារកិច្ចភ្ញៀវកិត្តិយសពិសារ ភោជនាអាហារដោយមេត្រីភាព", time: "ម៉ោង ០៥ : ០០​ ល្ងាច" }
]

// Computed
const getSpanClass = (index) => {
  if (index < 3) return 'col-span-4'
  if (index === 3) return 'col-span-12'
  if (index > 3 && index < 6) return 'col-span-6'
  return 'col-span-4'
}

// Methods
const openInvitation = () => {
  // Try to play music but don't block the invitation flow
  audio.value = new Audio('/songs/song.mp3')
  audio.value.play().catch(err => console.log('Audio play failed:', err))

  opened.value = true

  // Animated text transitions
  showSaveDate.value = true
  showEventDate.value = false

  setTimeout(() => {
    showSaveDate.value = false
    setTimeout(() => {
      showEventDate.value = true
    }, 400)
  }, 1500)

  // Show main invitation regardless of audio state
  setTimeout(() => {
    step.value = 2
    // Use nextTick to ensure DOM is ready before setting up observers
    setTimeout(() => {
      setupScrollObservers()
    }, 100)
  }, 6000)
}

const closeLightbox = () => {
  selectedImg.value = null
}

const setTimelineRef = (el, index, type) => {
  if (el) {
    if (!timelineRefs.value[index]) {
      timelineRefs.value[index] = {}
    }
    timelineRefs.value[index][type] = el
  }
}

const setGalleryRef = (el, index) => {
  if (el) {
    galleryRefs.value[index] = el
  }
}

const createObserver = (callback, threshold = 0.2) => {
  return new IntersectionObserver(callback, {
    threshold,
    rootMargin: '0px 0px -50px 0px'
  })
}

const setupScrollObservers = () => {
  // Small delay to ensure DOM is fully rendered
  setTimeout(() => {
    // Immediately show all main elements
    Object.keys(visibleElements.value).forEach(key => {
      visibleElements.value[key] = true
    })

    timelineHeaderVisible.value = true
    galleryTitleVisible.value = true

    // Observe main elements for future interactions
    const elementKeys = [
      'mainTitle', 'subtitle', 'parentsNames', 'mainCard',
      'invitationTitle', 'invitationText', 'coupleNames',
      'dateInfo', 'locationInfo', 'detailButton',
      'timelineSection', 'gallerySection'
    ]

    elementKeys.forEach(key => {
      const element = document.querySelector(`[data-ref="${key}"]`)
      if (element) {
        const observer = createObserver(([entry]) => {
          if (entry.isIntersecting) {
            visibleElements.value[key] = true
          }
        })
        observer.observe(element)
      }
    })

    // Observe timeline header
    const timelineHeader = document.querySelector('[data-ref="timelineHeader"]')
    if (timelineHeader) {
      const observer = createObserver(([entry]) => {
        if (entry.isIntersecting) timelineHeaderVisible.value = true
      })
      observer.observe(timelineHeader)
    }

    // Observe gallery title
    const galleryTitle = document.querySelector('[data-ref="galleryTitle"]')
    if (galleryTitle) {
      const observer = createObserver(([entry]) => {
        if (entry.isIntersecting) galleryTitleVisible.value = true
      })
      observer.observe(galleryTitle)
    }

    // Observe timeline items with staggered animation
    timelineRefs.value.forEach((refs, index) => {
      if (refs?.icon) {
        const observer = createObserver(([entry]) => {
          if (entry.isIntersecting) {
            setTimeout(() => {
              visibleItems.value[index] = true
            }, index * 100)
          }
        })
        observer.observe(refs.icon)
      }
    })

    // Observe gallery items with staggered animation
    galleryRefs.value.forEach((el, index) => {
      if (el) {
        const observer = createObserver(([entry]) => {
          if (entry.isIntersecting) {
            setTimeout(() => {
              visibleGalleryItems.value[index] = true
            }, index * 100)
          }
        })
        observer.observe(el)
      }
    })
  }, 100)
}

// Lifecycle
onMounted(() => {
  const urlParams = new URLSearchParams(window.location.search)
  const nameParam = urlParams.get('name')
  if (nameParam) {
    questName.value = decodeURIComponent(nameParam)
  }
})
</script>

<template>
  <div id="app">
    <!-- Landing Section -->
    <section v-if="!opened" class="landing">
      <div class="min-h-screen w-full flex flex-col justify-start px-4">
        <div class="fixed inset-0 z-0">
          <!-- <img src="/images/bg-curve.jpg" class="w-full h-full object-contain" alt="Background" /> -->
          <video autoplay muted loop playsinline class="w-full h-full object-cover">
            <source src="/images/vdo.mp4" type="video/mp4" />
          </video>
        </div>

        <div class="relative z-10 flex flex-col justify-center pt-20 items-center text-center">
          <div class="backdrop-blur-xs bg-white/10 rounded-[2.5rem] p-10 flex flex-col items-center
              text-center max-w-md shadow-2xl border border-white/30
              relative overflow-hidden">

            <div class="absolute inset-0 bg-linear-to-br from-white/10 to-transparent pointer-events-none"></div>
            <!-- <h1 class="text-2xl md:text-2xl font-moul leading-relaxed lg:text-4xl text-[#7B1F2A] text-center py-2"> -->
            <h1 class="text-lg gold-text sm:text-3xl whitespace-nowrap font-moul p-3 sm:p-4 rounded-lg">
              សិរីមង្គលអាពាហ៍ពិពាហ៍
            </h1>
            <p class="text-lg md:text-xl lg:text-2xl mb-8 text-[#ffffff] text-center max-w-md">
              The Wedding Day
            </p>
            <img src="/images/mark-png.svg" alt="Ornament" class="w-50 mb-8" />
            <h3 class="text-lg md:text-xl font-moul leading-relaxed lg:text-6xl mb-4 gold-text text-center">
              សូមគោរពអញ្ជើញ
            </h3>
            <div class="w-55 h-1 bg-white/60 mx-auto"></div>
            <p class="text-lg md:text-xl font-moul lg:text-2xl mb-2 mt-2 gold-text text-center max-w-md">
              {{ questName }}
            </p>
            <div class="w-55 h-1 bg-white/60 mx-auto mb-5"></div>
            <button class="group relative z-10 overflow-hidden px-10 py-4 rounded-full
             backdrop-blur-md bg-white/10 border border-white/40
             text-[#7B1F2A] font-nokora font-semibold text-lg
             shadow-[0_8px_32px_0_rgba(31,38,135,0.2)]
             hover:bg-white/20 hover:scale-105 transition-all duration-500 active:scale-95" @click="openInvitation">
              បើកធៀប
            </button>
          </div>
        </div>
      </div>
    </section>

    <!-- Story Splash -->
    <transition name="fade">
      <section v-if="opened && step === 1" class="story-screen">
        <div class="min-h-screen w-full flex flex-col justify-end items-center px-4 relative">
          <img src="/p1.jpg" class="absolute inset-0 w-full h-full object-cover" alt="Story background" />

          <div class="absolute inset-0 bg-linear-to-t from-black/80 via-black/40 to-transparent"></div>

          <div class="absolute bottom-0 left-0 right-0 z-10 pb-20">
            <div class="text-center px-4">
              <transition name="text-fade">
                <div v-if="showSaveDate" class="mb-8">
                  <p
                    class="font-save-date text-white text-4xl font-cinzel font-bold md:text-5xl uppercase tracking-[0.2em] mb-2">
                    Save the date
                  </p>
                  <div class="w-20 h-1 bg-white/60 mx-auto mt-4"></div>
                </div>
              </transition>

              <transition name="text-fade">
                <div v-if="showEventDate" class="animate-fade-in">
                  <p class="text-white/90 text-xl md:text-2xl font-moul mb-3">
                    ថ្ងៃអាទិត្យ ៧​ កើត​ ខែមាឃ
                  </p>
                  <p class="text-white text-5xl md:text-6xl font-moul mb-3">
                    ២៥
                  </p>
                  <p class="text-white/90 text-2xl md:text-3xl font-moul mb-2">
                    មករា ២០២៦
                  </p>
                  <div class="w-32 h-1 bg-white/60 mx-auto mt-6"></div>
                  <p class="text-white/80 text-lg md:text-xl font-nokora mt-4">
                    February 6, 2026
                  </p>
                </div>
              </transition>
            </div>
          </div>
        </div>
      </section>
    </transition>

    <!-- Main Invitation -->
    <section v-if="opened && step === 2" class="invitation">
      <div class="relative min-h-screen w-full flex flex-col justify-start items-center p-8">
        <div class="fixed inset-0 z-0">
          <img src="/images/bg-curve.jpg" class="w-full h-full object-contain" alt="Background" />
          <!-- <video autoplay muted loop playsinline class="w-full h-full object-cover">
            <source src="/images/vdo.mp4" type="video/mp4" />
          </video> -->
        </div>

        <div class="relative z-10 w-full px-4 flex flex-col justify-center pt-30 items-center text-center">
          <div class="backdrop-blur-xs bg-white/10 rounded-[2.5rem] p-10 flex flex-col items-center
              text-center max-w-md shadow-2xl border border-white/30
              relative overflow-hidden">
            <!-- <div class="absolute inset-0 bg-linear-to-br from-white/10 to-transparent pointer-events-none"></div> -->
            <h1 data-ref="mainTitle" :class="['text-[#7B1F2A] text-2xl md:text-2xl font-moul leading-relaxed lg:text-4xl text-center py-2 transition-all duration-1000',
              visibleElements.mainTitle ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-20']">
              សិរីមង្គលអាពាហ៍ពិពាហ៍
            </h1>

            <p data-ref="subtitle" :class="['text-lg md:text-xl lg:text-2xl mb-8 text-[#7B1F2A] text-center max-w-md transition-all duration-1000 delay-100',
              visibleElements.subtitle ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-20']">
              The Wedding Day
            </p>

            <div data-ref="parentsNames" :class="['text-[#7B1F2A] grid grid-cols-2 gap-4 font-moul leading-relaxed mb-4 text-center max-w-md transition-all duration-1000 delay-200',
              visibleElements.parentsNames ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-20']">
              <div class="grid grid-rows-2 gap-2">
                <p>លោក សាយ ថេត</p>
                <p> អ្នកស្រី ឡុង ដានេ</p>
              </div>
              <div class="grid grid-rows-2 gap-2">
                <p>លោក សេង ពិសាល</p>
                <p>អ្នកស្រី​ នូ វន្នី</p>
              </div>
            </div>

            <div data-ref="mainCard" :class="['transition-all duration-1000 delay-300',
              visibleElements.mainCard ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-20']">

              <h3 data-ref="invitationTitle" :class="['text-[#7B1F2A] text-lg md:text-lg font-moul leading-relaxed lg:text-xl mb-4 text-center transition-all duration-1000 delay-400',
                visibleElements.invitationTitle ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-20']">
                មានកិត្តិយសសូមគោរពអញ្ជើញ
              </h3>

              <p data-ref="invitationText" :class="['text-[#7B1F2A] font-metal leading-loose mb-4 transition-all duration-1000 delay-500',
                visibleElements.invitationText ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-20']">
                ឯកឧត្តម លោកឧកញ៉ា លោកជំទាវ លោក លោកស្រី អ្នកនាង កញ្ញា អញ្ជើញចូលរួមជាអធិបតី
                និងជាភ្ញៀវកិត្តិយសបើម្បីប្រសិទ្ធិពរជ័យ សិរីសួស្តីជ័យមង្គលក្នុងកម្មវិធីរៀបមង្គលអាពាហ៍ពិពាហ៍
                កូនប្រុស​-​កូនស្រី របស់យើងខ្ញុំ
              </p>

              <h2 data-ref="coupleNames" :class="['text-[#7B1F2A] font-moul text-lg leading-relaxed lg:text-5xl mb-4 text-center flex items-center justify-center gap-2 transition-all duration-1000 delay-600',
                visibleElements.coupleNames ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-20']">
                សន វិសាល
                <img src="/images/logo.png" alt="Logo" class="w-25" />
                សេង ស្រីនោ
              </h2>

              <p data-ref="dateInfo" :class="['text-[#7B1F2A] font-metal leading-loose mb-4 transition-all duration-1000 delay-700',
                visibleElements.dateInfo ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-20']">
                ដែលនឹងប្រព្រឹត្តទៅនៅថ្ងៃអាទិត្យ ៧កើត ខែមាឃ ឆ្នាំម្សាញ់ សប្តស័ក ពុទ្ធសករាជ ២៥៦៩ ត្រូវនឹងថ្ងៃទី ២៥ ខែមករា
                ឆ្នាំ ២០២៦
              </p>

              <p data-ref="locationInfo" :class="['text-[#7B1F2A] font-metal leading-loose mb-4 transition-all duration-1000 delay-800',
                visibleElements.locationInfo ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-20']">
                វេលាម៉ោង ៤: 00 រសៀល ​ស្ថិតនៅគេហដ្ឋានខាងស្រី ភូមិអណ្តូងថ្ម ឃុំគុស ស្រុកត្រាំកក់ ខេត្តតាកែវ។
                ដោយមេត្រីភាព!
              </p>


              <div data-ref="detailButton" :class="['mt-6 transition-all duration-1000 delay-900',
                visibleElements.detailButton ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-20']">
                <RouterLink to="/main-event"
                  class="bg-pink-600 text-white font-nokora px-6 py-2 md:px-8 md:py-3 rounded-full hover:bg-pink-700 hover:scale-105 transition-all duration-300 font-semibold text-lg shadow-lg">
                  ព័ត៌មានលម្អិត
                </RouterLink>
              </div>

              <!-- Timeline Section -->
              <div data-ref="timelineSection" :class="['p-6 sm:p-8 md:p-12 bg-transparent transition-all duration-1000 delay-1000',
                visibleElements.timelineSection ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-20']">
                <h2 data-ref="timelineHeader" :class="['text-lg sm:text-xl text-center whitespace-nowrap font-moul text-[#7B1F2A] bg-white/10 p-3 sm:p-4 rounded-lg transition-all duration-1000',
                  timelineHeaderVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-20']">
                  កម្មវិធីមង្គលអាពាហ៍ពិពាហ៍
                </h2>

                <div class="relative">
                  <div class="absolute left-1/2 transform -translate-x-1/2 w-0.5 bg-secondary dark:bg-primary/30 h-full">
                  </div>

                  <div class="grid grid-cols-[auto_1fr] gap-x-4 -gap-y-2 text-left">
                    <template v-for="(event, index) in timelineEvents" :key="index">
                      <div :ref="el => setTimelineRef(el, index, 'icon')" :class="['flex flex-col items-left gap-1 pt-3 relative transition-all duration-700',
                        visibleItems[index] ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-20']">
                        <div class="text-primary text-3xl">
                          <img :src="event.icon" alt="event icon"
                            class="w-18 sm:w-12 md:w-16 rounded-xl bg-transparent mix-blend-multiply" />
                        </div>
                        <div class="w-0.5 bg-secondary dark:bg-primary/30 h-2"></div>
                      </div>
                      <div :class="['flex flex-1 flex-col pb-6 pt-2 transition-all duration-700 delay-150',
                        visibleItems[index] ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-20']">
                        <p class="text-text-light font-nokora text-[#7B1F2A] text-lg font-semibold leading-normal">
                          {{ event.title }}
                        </p>
                        <p class="text-base text-[#e73a6b] font-moul leading-normal">{{ event.time }}</p>
                      </div>
                    </template>
                  </div>
                </div>
              </div>

              <!-- Gallery Section -->
              <div data-ref="gallerySection" :class="['relative transition-all duration-1000 delay-1100',
                visibleElements.gallerySection ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-20']">
                <h2 data-ref="galleryTitle" :class="['text-lg sm:text-xl text-center whitespace-nowrap font-moul text-[#7B1F2A] bg-white/10 p-3 sm:p-4 rounded-lg transition-all duration-1000',
                  galleryTitleVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-20']">
                  កម្រងរូបភាព
                </h2>

                <div class="grid grid-cols-12 gap-4">
                  <div v-for="(img, index) in images" :key="img.id" :ref="el => setGalleryRef(el, index)" :class="['relative overflow-hidden rounded-lg cursor-pointer group transition-all duration-700',
                    getSpanClass(index),
                    visibleGalleryItems[index] ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-20']"
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
                      class="fixed inset-0 z-1000 bg-black/90 backdrop-blur-sm flex items-center justify-center p-4"
                      @click="closeLightbox">
                      <button class="absolute top-6 right-6 text-white text-4xl hover:text-gray-300 transition"
                        @click="closeLightbox">
                        &times;
                      </button>
                      <img :src="selectedImg"
                        class="max-w-full max-h-[90vh] object-contain rounded-lg shadow-2xl transition-all" @click.stop
                        alt="Full size image" />
                      <p class="absolute bottom-6 text-white/60 text-sm">Click anywhere to close</p>
                    </div>
                  </transition>
                </Teleport>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
.gold-text {
  background: linear-gradient(to bottom, #FFED4E 0%, #FFD700 40%, #FFA500 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  filter:
  drop-shadow(0 1px 2px rgba(0, 0, 0, 0.4))
          drop-shadow(0 2px 4px rgba(36, 29, 29, 0.3))
          drop-shadow(0 0 12px rgba(241, 214, 59, 0.5));
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.text-fade-enter-active {
  transition: opacity 0.8s ease;
}

.text-fade-leave-active {
  transition: opacity 0.8s ease;
}

.text-fade-enter-from {
  opacity: 0;
}

.text-fade-leave-to {
  opacity: 0;
}

.animate-fade-in {
  animation: fadeInUp 1s ease-out;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.delay-100 {
  transition-delay: 100ms;
}

.delay-150 {
  transition-delay: 150ms;
}

.delay-200 {
  transition-delay: 200ms;
}

.delay-300 {
  transition-delay: 300ms;
}

.delay-400 {
  transition-delay: 400ms;
}

.delay-500 {
  transition-delay: 500ms;
}

.delay-600 {
  transition-delay: 600ms;
}

.delay-700 {
  transition-delay: 700ms;
}

.delay-800 {
  transition-delay: 800ms;
}

.delay-900 {
  transition-delay: 900ms;
}

.delay-1000 {
  transition-delay: 1000ms;
}

.delay-1100 {
  transition-delay: 1100ms;
}
</style>
