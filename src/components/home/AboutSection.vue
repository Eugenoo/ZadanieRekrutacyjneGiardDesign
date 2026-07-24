<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import aboutImage from '../../assets/photos/about.png'

// Refy do sterowania animacją wejścia
const sectionRef = ref<HTMLElement | null>(null)
const isVisible = ref(false)

let observer: IntersectionObserver | null = null

onMounted(() => {
  // Intersection Observer – wykrywa, gdy sekcja pojawi się w 20% w obszarze widzenia
  observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          isVisible.value = true
          // Po jednorazowym aktywowaniu można przestać obserwować
          if (sectionRef.value) observer?.unobserve(sectionRef.value)
        }
      },
      { threshold: 0.2 }
  )

  if (sectionRef.value) {
    observer.observe(sectionRef.value)
  }
})

onUnmounted(() => {
  if (observer) observer.disconnect()
})
</script>

<template>
  <section
      ref="sectionRef"
      class="bg-green-custom w-full relative overflow-hidden min-h-[600px] lg:min-h-[720px] lg:h-[720px] flex items-center"
  >

    <!-- LEWA STRONA (TŁO): Zdjęcie przyklejone do lewej krawędzi (0 do 50vw) -->
    <!-- Dodano: group dla efektu hover oraz animację wejścia z lewej (Translate X) -->
    <div
        class="hidden lg:block absolute top-0 left-0 w-1/2 h-full z-0 group overflow-hidden transition-all duration-1000 ease-out"
        :class="[
        isVisible ? 'translate-x-0 opacity-100' : '-translate-x-12 opacity-0'
      ]"
    >
      <img
          :src="aboutImage"
          alt="O firmie - Tworzymy z pasją"
          class="w-full h-full object-cover transition-transform duration-700 ease-out group-hover:scale-105"
      />
    </div>

    <!-- SIATKA GŁÓWNA (1440px wyśrodkowana na ekranie) -->
    <div class="w-full max-w-[1440px] mx-auto h-full grid grid-cols-1 lg:grid-cols-2 relative z-10">

      <!-- Wersja mobilna zdjęcia z zoomem oraz animacją -->
      <div
          class="w-full h-[350px] sm:h-[450px] lg:h-full lg:opacity-0 overflow-hidden group transition-all duration-1000 ease-out"
          :class="[
          isVisible ? 'translate-x-0 opacity-100' : '-translate-x-8 opacity-0'
        ]"
      >
        <img
            :src="aboutImage"
            alt="O firmie - Tworzymy z pasją"
            class="w-full h-full object-cover lg:hidden transition-transform duration-700 ease-out group-hover:scale-105"
        />
      </div>

      <!-- PRAWA STRONA: Animowane ujawnianie bloku tekstowego z prawej strony -->
      <div class="flex items-center justify-end lg:ml-16 h-full py-6">

        <!-- BLOK TREŚCI: Przesunięcie z prawej (translate-x) przy wejściu -->
        <div
            class="w-full max-w-[596px] h-auto lg:h-[450px] flex  ml-6 md:ml-24 flex-col justify-between gap-8 lg:gap-16 transition-all duration-1000 delay-150 ease-out"
            :class="[
            isVisible ? 'translate-x-0 opacity-100' : 'translate-x-12 opacity-0'
          ]"
        >

          <div class="flex flex-col gap-4">
            <p class="text-stone-100 text-xs tracking-wider font-light">
              O firmie
            </p>

            <div class="flex flex-col gap-6 lg:gap-10">
              <h2 class="text-stone-100 text-3xl sm:text-4xl lg:text-5xl font-medium leading-tight lg:leading-[55px] font-['Montserrat']">
                <p>Tworzymy</p>
                <p class="italic">z pasją</p>
              </h2>

              <p class="text-stone-100 text-sm lg:text-base leading-relaxed font-['Inter'] opacity-90 pr-4">
                Każdy projekt to nowe wyzwanie. Dlatego nasz zespół tworzą
                wykwalifikowani projektanci oraz architekci, których zadaniem
                jest rozpoznanie i realizacja potrzeb każdego Klienta.
                Nasza specjalizacja to przestrzenie nowoczesne, które
                charakteryzuje minimalizm, geometria i elegancka prostota.
                Tworzymy ogrody małoobsługowe, dostosowane do współczesnego
                trybu życia.
              </p>
            </div>
          </div>

          <!-- PRZYCISK: Dodatkowo delikatnie przesuwa się w prawo na hover -->
          <a
              href="#contact"
              class="mx-auto md:mx-0 group px-6 py-3.5 rounded-full border border-stone-100 text-stone-100 flex items-center gap-3 w-fit text-sm font-medium transition-all duration-300 hover:bg-stone-100 hover:text-green-900 font-['Inter'] hover:translate-x-1"
          >
            <span class="transition-transform duration-300 group-hover:translate-x-0.5">Poznaj nas bliżej</span>

            <svg
                xmlns="http://www.w3.org/2000/svg"
                class="w-4 h-4 transition-transform duration-300 -rotate-90 group-hover:translate-x-1.5"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
                stroke-width="2"
            >
              <path stroke-linecap="round" stroke-linejoin="round" d="M19 9l-7 7-7-7" />
            </svg>
          </a>

        </div>

      </div>

    </div>
  </section>
</template>