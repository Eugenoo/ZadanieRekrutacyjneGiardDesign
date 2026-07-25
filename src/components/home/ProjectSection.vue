<script setup>
import { ref, computed, onMounted, nextTick } from 'vue'
import Masonry from 'masonry-layout'
import imagesLoaded from 'imagesloaded'

// 1. IMPORT FANCYBOX I JEGO STYLÓW
import { Fancybox } from '@fancyapps/ui'
import '@fancyapps/ui/dist/fancybox/fancybox.css'

const isExpanded = ref(false)
const sectionRef = ref(null)

const toggleExpand = async () => {
  isExpanded.value = !isExpanded.value
  await nextTick()

  if (msnry) {
    msnry.layout()
  }

  // JEŚLI WŁAŚNIE ZWINĘLIŚMY SEKCJĘ:
  if (!isExpanded.value && sectionRef.value) {
    // Czekamy chwilę, aż animacja zwijania ruszy / dobiegnie końca
    setTimeout(() => {
      // Pobieramy pozycję top naszej sekcji
      const sectionTop = sectionRef.value.getBoundingClientRect().top + window.scrollY

      // Płynny scroll do góry sekcji z lekkim zapasem (np. -100px na padding/header)
      window.scrollTo({
        top: sectionTop - 50,
        behavior: 'smooth'
      })
    }, 400) // 400ms – świetny moment w trakcie/pod koniec zwijania
  }
}

const rawProjects = [
  { id: 1, image: '/projects/photo-1.png', alt: 'Projekt 1' },
  { id: 2, image: '/projects/photo-2.png', alt: 'Projekt 2' },
  { id: 3, image: '/projects/photo-3.png', alt: 'Projekt 3' },
  { id: 4, image: '/projects/photo-4.png', alt: 'Projekt 4' },
  { id: 5, image: '/projects/photo-5.png', alt: 'Projekt 5' },
  { id: 6, image: '/projects/photo-6.png', alt: 'Projekt 6' },
  { id: 7, image: '/projects/photo-7.png', alt: 'Projekt 7' },
  { id: 8, image: '/projects/photo-8.png', alt: 'Projekt 8' },
  { id: 9, image: '/projects/photo-9.png', alt: 'Projekt 9' },
]

const projects = computed(() => {
  const doubleList = [...rawProjects, ...rawProjects]
  return doubleList.map((project, index) => ({
    ...project,
    uniqueId: `${project.id}-${index}`
  }))
})

const gridContainer = ref(null)
let msnry = null

const initMasonry = () => {
  if (!gridContainer.value) return

  imagesLoaded(gridContainer.value, () => {
    if (!msnry) {
      msnry = new Masonry(gridContainer.value, {
        itemSelector: '.grid-item',
        columnWidth: '.grid-sizer',
        percentPosition: true,
        gutter: 32
      })
    } else {
      msnry.reloadItems()
      msnry.layout()
    }
  })
}

onMounted(() => {
  initMasonry()

  // 2. INICJALIZACJA GALERII FANCYBOX
  Fancybox.bind('[data-fancybox="gallery"]', {
    loop: true,
    Hash: false,
  })
})
</script>

<template>
  <section ref="sectionRef" class="relative bg-orange-custom w-full mx-auto pt-32 pb-20">

    <!-- Nagłówek -->
    <div class="flex flex-col gap-4 mb-24 relative z-10 px-10 md:px-[140px]">
      <p class="text-green-900 text-xs font-semibold uppercase tracking-wider">
        Realizacje
      </p>
      <h2 class="text-neutral-900 text-5xl font-medium">
        <span class="font-heading">Nasze </span>  <span class="italic">projekty</span>
      </h2>
    </div>

    <!-- Kontener z rozwijaniem (spowolniony duration-[1400ms]) -->
    <div
        class="w-full overflow-hidden transition-[max-height] duration-[1400ms] ease-[cubic-bezier(0.25,1,0.3,1)] relative z-10"
        :class="isExpanded ? 'max-h-[5000px]' : 'max-h-[1600px]'"
    >
      <!-- Kontener Masonry -->
      <div ref="gridContainer" class="w-full">
        <div class="grid-sizer w-full md:w-[calc(33.333%-22px)]"></div>

        <div
            v-for="project in projects"
            :key="project.uniqueId"
            class="grid-item w-full md:w-[calc(33.333%-22px)] mb-8"
        >
          <div class="overflow-hidden bg-neutral-100 shadow-sm transition-transform duration-300 hover:-translate-y-1">

            <!-- OWINIĘCIE OBRAZKA W LINK FANCYBOX -->
            <a
                :href="project.image"
                data-fancybox="gallery"
                :data-caption="project.alt || 'Zdjęcie projektu'"
                class="block cursor-zoom-in"
            >
              <img
                  :src="project.image"
                  :alt="project.alt || 'Zdjęcie projektu'"
                  class="w-full h-auto object-cover block"
              />
            </a>

          </div>
        </div>
      </div>
    </div>

    <!-- Gradient ze spowolnionym wygasaniem -->
    <div
        class="absolute bottom-0 left-0 right-0 z-20 pointer-events-none bg-gradient-to-t from-[#F8ECE0] via-[#F8ECE0]/80 to-transparent transition-all duration-[1000ms] ease-in-out"
        :class="isExpanded
        ? 'h-[400px] opacity-100 md:h-[250px] md:opacity-40'
        : 'h-[1000px] opacity-100'"
    ></div>

    <!-- Przycisk Rozwiń / Zwiń -->
    <div class="absolute bottom-8 left-1/2 -translate-x-1/2 z-30">
      <button
          @click="toggleExpand"
          class="flex items-center gap-3 px-8 py-4 bg-transparent hover:bg-black hover:text-white text-black border-2 border-black font-medium rounded-full transition-all duration-300 hover:scale-105 active:scale-95 cursor-pointer"
      >
        <span>{{ isExpanded ? 'Zwiń' : 'Rozwiń' }}</span>

        <!-- Strzałka z trzonkiem i grotem (3 linie) -->
        <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="2"
            stroke="currentColor"
            class="w-5 h-5 transition-transform duration-500 ease-in-out"
            :class="{ 'rotate-180': isExpanded }"
        >
          <path stroke-linecap="round" stroke-linejoin="round" d="M12 4.5v15m0 0l6-6m-6 6l-6-6" />
        </svg>
      </button>
    </div>
  </section>
</template>