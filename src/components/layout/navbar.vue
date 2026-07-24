<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

// --- 1. Obsługa ukrywania / pokazywania nawigacji przy scrollu ---
const isVisible = ref(true)
let lastScrollY = 0

const handleScroll = () => {
  const currentScrollY = window.scrollY

  // Pokazuj navbar na samej górze strony, chowaj przy scrollowaniu w dół
  if (currentScrollY <= 0) {
    isVisible.value = true
  } else if (currentScrollY > lastScrollY && currentScrollY > 72) {
    // Scroll w dół (poza wysokość nawigacji) -> schowaj
    isVisible.value = false
    isDropdownOpen.value = false // opcjonalnie zamykamy menu przy scrollu
  } else if (currentScrollY < lastScrollY) {
    // Scroll w górę -> pokaż
    isVisible.value = true
  }

  lastScrollY = currentScrollY
}

// --- 2. Obsługa Dropdownu dla "Oferta" ---
const isDropdownOpen = ref(false)
const dropdownRef = ref<HTMLElement | null>(null)

const toggleDropdown = () => {
  isDropdownOpen.value = !isDropdownOpen.value
}

// Zamykanie dropdownu po kliknięciu poza niego
const handleClickOutside = (event: MouseEvent) => {
  if (dropdownRef.value && !dropdownRef.value.contains(event.target as Node)) {
    isDropdownOpen.value = false
  }
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll, { passive: true })
  document.addEventListener('click', handleClickOutside)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
  document.removeEventListener('click', handleClickOutside)
})
</script>

<template>
  <nav
      class="fixed top-0 left-0 z-50 h-[72px] w-full border-b border-gray-200 bg-white transition-transform duration-300 ease-in-out"
      :class="isVisible ? 'translate-y-0' : '-translate-y-full'"
  >
    <div class="mx-auto flex h-full max-w-[1262px] items-center justify-between px-4">
      <!-- Logo -->
      <div class="text-xl font-bold text-neutral-900">
        <a href="/">
          <img src="../../assets/logo.svg" alt="logo" />
        </a>
      </div>

      <!-- Menu DESKTOP -->
      <div class="hidden items-center gap-12 md:flex">

        <!-- Dropdown Oferta -->
        <div ref="dropdownRef" class="relative">
          <button
              @click="toggleDropdown"
              class="flex items-center gap-1 text-sm font-normal text-neutral-900 hover:text-neutral-600 focus:outline-none"
              :aria-expanded="isDropdownOpen"
          >
            Oferta
            <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-4 w-4 transition-transform duration-200"
                :class="{ 'rotate-180': isDropdownOpen }"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
                stroke-width="2"
            >
              <path stroke-linecap="round" stroke-linejoin="round" d="M19 9l-7 7-7-7" />
            </svg>
          </button>

          <!-- Zawartość Dropdownu -->
          <Transition
              enter-active-class="transition duration-150 ease-out"
              enter-from-class="transform scale-95 opacity-0"
              enter-to-class="transform scale-100 opacity-100"
              leave-active-class="transition duration-100 ease-in"
              leave-from-class="transform scale-100 opacity-100"
              leave-to-class="transform scale-95 opacity-0"
          >
            <div
                v-if="isDropdownOpen"
                class="absolute left-0 mt-3 w-56 rounded-md border border-gray-100 bg-white p-2 shadow-lg ring-1 ring-black/5"
            >
              <a
                  href="#offer"
                  class="block rounded-md px-4 py-2 text-sm text-neutral-700 hover:bg-neutral-100 hover:text-neutral-900"
              >
                Projekty
              </a>
              <a
                  href="#offer"
                  class="block rounded-md px-4 py-2 text-sm text-neutral-700 hover:bg-neutral-100 hover:text-neutral-900"
              >
                Wizualizacje
              </a>
              <a
                  href="#offer"
                  class="block rounded-md px-4 py-2 text-sm text-neutral-700 hover:bg-neutral-100 hover:text-neutral-900"
              >
                Realizacje
              </a>
            </div>
          </Transition>
        </div>

        <a href="#about" class="text-sm font-normal text-neutral-900 hover:text-neutral-600">
          O firmie
        </a>

        <a href="#realisations" class="text-sm font-normal text-neutral-900 hover:text-neutral-600">
          Realizacje
        </a>

        <a href="#contact" class="text-sm font-normal text-neutral-900 hover:text-neutral-600">
          Kontakt
        </a>

        <a href="/szukaj" aria-label="Szukaj">
          <svg
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
          >
            <circle cx="11" cy="11" r="8"></circle>
            <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
          </svg>
        </a>
      </div>

      <!-- MOBILE -->
      <button class="text-3xl md:hidden">
        ☰
      </button>
    </div>
  </nav>
</template>

<style scoped>
</style>