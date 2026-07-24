<script setup lang="ts">
import { ref } from 'vue'

defineProps<{
  title: string
  text: string
  linkText?: string
  href?: string
  icon?: string
}>()

const cardRef = ref<HTMLElement | null>(null)
const rotateX = ref(0)
const rotateY = ref(0)
const isHovered = ref(false)

const handleMouseMove = (event: MouseEvent) => {
  if (!cardRef.value) return

  const rect = cardRef.value.getBoundingClientRect()
  const x = event.clientX - rect.left
  const y = event.clientY - rect.top

  const centerX = rect.width / 2
  const centerY = rect.height / 2

  const maxTilt = 10

  rotateX.value = -((y - centerY) / centerY) * maxTilt
  rotateY.value = ((x - centerX) / centerX) * maxTilt
}

const handleMouseEnter = () => {
  isHovered.value = true
}

const handleMouseLeave = () => {
  isHovered.value = false
  rotateX.value = 0
  rotateY.value = 0
}
</script>

<template>
  <!-- Kontener perspektywy 3D -->
  <div class="perspective-1000 h-full w-full">
    <!--
      WRAPER RAMKI:
      - `p-[2px]` tworzy idealną obramówkę o grubości 2px wokół całej karty.
      - Na hover aktywuje obracający się promieniście gradient pod spodem.
    -->
    <div
        ref="cardRef"
        @mouseenter="handleMouseEnter"
        @mousemove="handleMouseMove"
        @mouseleave="handleMouseLeave"
        :style="{
        transform: `rotateX(${rotateX}deg) rotateY(${rotateY}deg)`,
        transition: isHovered ? 'none' : 'transform 0.5s ease, box-shadow 0.3s ease'
      }"
        class="glow-wrapper group relative h-full w-full rounded-3xl p-[2px] cursor-pointer transform-gpu ease-out shadow-sm hover:shadow-2xl overflow-hidden bg-neutral-200/60"
    >
      <!-- ANIMOWANY ŚWIECĄCY STOŻEK (Tylko na krawędzi 2px) -->
      <div
          class="glow-spinner absolute inset-[-100%] opacity-0 group-hover:opacity-100 transition-opacity duration-500 pointer-events-none"
      ></div>

      <!-- KARTA WŁAŚCIWA (A) - zakrywa środek, pozostawiając p-[2px] podświetlonej ramki -->
      <a
          :href="href || '#'"
          class="relative flex h-full w-full flex-col justify-between rounded-[calc(1.5rem-2px)] bg-white p-10 z-10"
      >
        <div>
          <div class="flex transition-all duration-300 group-hover:text-green-900">
            <img v-if="icon" :src="icon" :alt="title" class="h-8 transition-all duration-300" />
          </div>

          <!-- Nagłówek z akcentem kolorystycznym -->
          <div class="relative mt-8 inline-block">
            <h3 class="text-2xl font-semibold text-neutral-900 font-['Montserrat']">
              {{ title }}
            </h3>

            <!-- ANIMOWANY AKCENT: Linia rozciągająca się od środka -->
            <span
                class="absolute -bottom-1 left-0 h-[2px] w-full bg-green-900 origin-center scale-x-0 transition-transform duration-300 ease-out group-hover:scale-x-100"
            ></span>
          </div>

          <p class="mt-4 text-base leading-relaxed text-neutral-600 font-['Inter']">
            {{ text }}
          </p>
        </div>

        <!-- Przycisk/Link na dole z animowaną strzałką na hover -->
        <div class="mt-12 flex items-center gap-2 text-sm font-medium text-green-900 group-hover:text-green-800">
          <span class="border-b border-green-900 group-hover:border-green-800 pb-0.5">
            {{ linkText || 'Dowiedz się więcej' }}
          </span>
          <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-4 w-4 transition-transform duration-300 group-hover:translate-x-1.5"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              stroke-width="2"
          >
            <path stroke-linecap="round" stroke-linejoin="round" d="M17.25 8.25L21 12m0 0l-3.75 3.75M21 12H3" />
          </svg>
        </div>
      </a>
    </div>
  </div>
</template>

<style scoped>
.perspective-1000 {
  perspective: 1000px;
}

/* Obracający się stożek zielonego światła */
.glow-spinner {
  background: conic-gradient(
      from var(--shimmer-angle),
      transparent 0%,
      transparent 30%,
      #15803d 50%, /* Zielony akcent Tailwind (green-700) */
      transparent 70%,
      transparent 100%
  );
  animation: shimmer 2s linear infinite;
}

@property --shimmer-angle {
  syntax: '<angle>';
  initial-value: 0deg;
  inherits: false;
}

@keyframes shimmer {
  0% { --shimmer-angle: 0deg; }
  100% { --shimmer-angle: 360deg; }
}
</style>