<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import { Swiper, SwiperSlide } from 'swiper/vue'
import { Navigation, Autoplay } from 'swiper/modules'

// Style Swipera
import 'swiper/css'

const modules = [Navigation, Autoplay]

const slides = [
  {
    id: 1,
    title: 'Nowoczesna aranżacja <br /> Twojego ogrodu',
    description: 'Marka GiardDesign to wieloletnie doświadczenie i wysoka estetyka realizacji. Oferujemy kompleksowy zakres usług z indywidualnym podejściem do każdego projektu.',
    image: '../../../src/assets/photos/hero.png',
    alt: 'Ogród 1'
  },
  {
    id: 2,
    title: 'Unikalny projekt <br /> dopasowany do Ciebie',
    description: 'Tworzymy przestrzenie, które zachwycają i pozwalają na pełen relaks w domowym zaciszu. Sprawdź naszą ofertę.',
    image: '../../../src/assets/photos/hero.png',
    alt: 'Ogród 2'
  }
]

// Efekt Parallax na scrollowanie strony
const scrollY = ref(0)

const handleScroll = () => {
  scrollY.value = window.scrollY
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll, { passive: true })
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<template>
  <section class="bg-orange-custom overflow-hidden md:h-[calc((100vh-72px)*6/7)] min-h-[600px] w-full relative flex flex-col">
    <Swiper
        :modules="modules"
        :loop="true"
        :speed="700"
        :autoplay="{ delay: 5000, disableOnInteraction: false }"
        :navigation="{
        nextEl: '#hero-next-btn',
        prevEl: '#hero-prev-btn'
      }"
        class="w-full h-full flex-1 !flex hero-swiper"
    >
      <SwiperSlide v-for="slide in slides" :key="slide.id" class="!h-full flex">
        <div class="grid grid-cols-1 md:grid-cols-2 w-full h-full">

          <!-- LEWA KOLUMNA: Tekst i animowana typografia -->
          <div class="order-2 md:order-1 flex items-center justify-end md:py-6 px-4 py-20 md:pl-8 lg:pl-16 md:pr-12 z-10">
            <div class="w-full max-w-[550px] flex flex-col justify-center">

              <!-- Animacje ujawniania z blur-to-clear przy zmianie slajdu -->
              <div class="space-y-4 lg:space-y-6">
                <h1
                    class="font-heading slide-anim-title text-3xl lg:text-5xl xl:text-6xl leading-tight lg:leading-[1.12] text-neutral-900"
                    v-html="slide.title"
                ></h1>

                <p class="slide-anim-desc max-w-[490px] text-sm lg:text-base leading-relaxed text-neutral-900 font-['Inter']">
                  {{ slide.description }}
                </p>
              </div>

              <!-- Przyciski CTA -->
              <div class="slide-anim-cta mt-6 lg:mt-10 flex flex-wrap items-center gap-4 lg:gap-8">

                <a href="#contact">
                  <button
                      class="group relative inline-flex items-center justify-center overflow-hidden rounded-full bg-green-900 px-7 py-3.5 text-sm lg:text-base font-medium text-stone-100 shadow-md transition-all duration-300 ease-out hover:scale-105 hover:bg-green-800 hover:shadow-lg hover:shadow-green-900/30 active:scale-95"
                  >
                    <span class="relative z-10">Skontaktuj się z nami</span>
                  </button>
                </a>


                <!-- Przycisk Drugorzędny z płynnym przesuwem koloru -->
                <a href="#realisations">
                  <button
                      class="group relative overflow-hidden rounded-full border border-green-900 px-6 py-3.5 text-sm lg:text-base font-medium text-green-900 transition-colors duration-300 active:scale-95"
                  >
                    <span class="absolute inset-0 bg-green-900 transition-transform duration-300 ease-out -translate-x-full group-hover:translate-x-0"></span>
                    <span class="relative z-10 flex items-center gap-2 transition-colors duration-300 group-hover:text-white">
                    Zobacz nasze realizacje
                    <svg
                        xmlns="http://www.w3.org/2000/svg"
                        class="h-4 w-4 -rotate-90 transition-transform duration-300 group-hover:translate-x-1"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                        stroke-width="2"
                    >
                      <path stroke-linecap="round" stroke-linejoin="round" d="M19 9l-7 7-7-7" />
                    </svg>
                  </span>
                  </button>
                </a>

              </div>
            </div>
          </div>

          <!-- PRAWA KOLUMNA: Subtelny Parallax na tle -->
          <div class="order-1 md:order-2 relative h-60 md:h-full w-full bg-gray-300 overflow-hidden">
            <div
                class="absolute inset-0 w-full h-[120%] -top-[10%] transition-transform ease-out duration-75"
                :style="{ transform: `translate3d(0, ${scrollY * 0.15}px, 0)` }"
            >
              <img
                  :src="slide.image"
                  :alt="slide.alt"
                  class="h-full w-full object-cover"
              />
            </div>

            <!-- Nawigacja slidera w prawym dolnym rogu -->
            <div class="absolute bottom-0 right-0 flex bg-stone-100 z-20 shadow-md">
              <button
                  id="hero-prev-btn"
                  aria-label="Poprzedni slajd"
                  class="flex h-14 w-14 lg:h-20 lg:w-20 items-center justify-center text-xl text-neutral-800 transition-colors duration-200 hover:bg-neutral-200 active:bg-neutral-300"
              >
                ←
              </button>

              <button
                  id="hero-next-btn"
                  aria-label="Następny slajd"
                  class="flex h-14 w-14 lg:h-20 lg:w-20 items-center justify-center text-xl text-neutral-800 transition-colors duration-200 hover:bg-neutral-200 active:bg-neutral-300"
              >
                →
              </button>
            </div>
          </div>

        </div>
      </SwiperSlide>
    </Swiper>
  </section>
</template>

<style scoped>
/* Animacje ujawniania tekstu przy aktywacji slajdu */
.hero-swiper :deep(.swiper-slide-active) .slide-anim-title {
  animation: slideFadeUp 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}

.hero-swiper :deep(.swiper-slide-active) .slide-anim-desc {
  animation: slideFadeUp 0.8s cubic-bezier(0.16, 1, 0.3, 1) 0.12s forwards;
}

.hero-swiper :deep(.swiper-slide-active) .slide-anim-cta {
  animation: slideFadeUp 0.8s cubic-bezier(0.16, 1, 0.3, 1) 0.24s forwards;
}

.slide-anim-title,
.slide-anim-desc,
.slide-anim-cta {
  opacity: 0;
  transform: translateY(20px);
  filter: blur(6px);
}

@keyframes slideFadeUp {
  0% {
    opacity: 0;
    transform: translateY(20px);
    filter: blur(6px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
    filter: blur(0);
  }
}
</style>