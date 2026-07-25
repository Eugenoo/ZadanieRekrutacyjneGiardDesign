<script setup lang="ts">
import { ref, onMounted, onUnmounted, nextTick } from 'vue'

// --- 1. Obsługa ukrywania / pokazywania nawigacji przy scrollu ---
const isVisible = ref(true)
let lastScrollY = 0

const handleScroll = () => {
  const currentScrollY = window.scrollY

  if (currentScrollY <= 0) {
    isVisible.value = true
  } else if (currentScrollY > lastScrollY && currentScrollY > 72) {
    isVisible.value = false
    isDropdownOpen.value = false
    isMobileMenuOpen.value = false // Zamykamy menu mobilne przy scrollu w dół
    if (!searchQuery.value) isSearchExpanded.value = false
  } else if (currentScrollY < lastScrollY) {
    isVisible.value = true
  }

  lastScrollY = currentScrollY
}

// --- 2. Obsługa Dropdownu dla "Oferta" (Desktop) ---
const isDropdownOpen = ref(false)
const dropdownRef = ref<HTMLElement | null>(null)

const toggleDropdown = () => {
  isDropdownOpen.value = !isDropdownOpen.value
}

// --- 2.1. Obsługa menu Mobilnego (Hamburger + Overlay) ---
const isMobileMenuOpen = ref(false)
const isMobileOfferOpen = ref(false) // Dropdown wewnątrz menu mobilnego

const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value
  // Blokowanie przewijania strony, gdy menu jest otwarte
  if (isMobileMenuOpen.value) {
    document.body.style.overflow = 'hidden'
  } else {
    document.body.style.overflow = ''
    isMobileOfferOpen.value = false
  }
}

const closeMobileMenu = () => {
  isMobileMenuOpen.value = false
  document.body.style.overflow = ''
  isMobileOfferOpen.value = false
}

// --- 3. Obsługa Szalonego Search Bara ---
const isSearchExpanded = ref(false)
const searchQuery = ref('')
const searchInput = ref<HTMLInputElement | null>(null)
const searchContainerRef = ref<HTMLElement | null>(null)

const toggleSearch = async () => {
  isSearchExpanded.value = !isSearchExpanded.value

  if (isSearchExpanded.value) {
    await nextTick()
    searchInput.value?.focus()
  }
}

const closeSearch = () => {
  if (!searchQuery.value) {
    isSearchExpanded.value = false
  }
}

const handleSearch = () => {
  if (searchQuery.value.trim() !== '') {
    alert(`Wyszukujesz: ${searchQuery.value}`)
    searchQuery.value = ''
    isSearchExpanded.value = false
  }
}

// Zamykanie dropdownu i search bara po kliknięciu poza nie
const handleClickOutside = (event: MouseEvent) => {
  const target = event.target as Node

  if (dropdownRef.value && !dropdownRef.value.contains(target)) {
    isDropdownOpen.value = false
  }

  if (
      searchContainerRef.value &&
      !searchContainerRef.value.contains(target) &&
      !searchQuery.value
  ) {
    isSearchExpanded.value = false
  }
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll, { passive: true })
  document.addEventListener('click', handleClickOutside)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
  document.removeEventListener('click', handleClickOutside)
  document.body.style.overflow = '' // Reset przy demontażu
})
</script>

<template>
  <nav
      class="fixed top-0 left-0 z-50 h-[72px] w-full border-b border-gray-200 bg-white transition-transform duration-300 ease-in-out"
      :class="isVisible ? 'translate-y-0' : '-translate-y-full'"
  >
    <div class="mx-auto flex h-full max-w-[1262px] items-center justify-between px-4">
      <!-- Logo -->
      <div class="text-xl font-bold text-neutral-900 flex-shrink-0 z-50">
        <a href="/">
          <img src="../../assets/logo.svg" alt="logo" />
        </a>
      </div>

      <!-- Menu DESKTOP -->
      <div class="hidden items-center gap-12 md:flex">
        <!-- Dropdown Oferta -->
        <div ref="dropdownRef" class="relative whitespace-nowrap">
          <button
              @click.stop="toggleDropdown"
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
              <a href="#offer" class="block rounded-md px-4 py-2 text-sm text-neutral-700 hover:bg-neutral-100 hover:text-neutral-900">
                Projekty
              </a>
              <a href="#offer" class="block rounded-md px-4 py-2 text-sm text-neutral-700 hover:bg-neutral-100 hover:text-neutral-900">
                Wizualizacje
              </a>
              <a href="#offer" class="block rounded-md px-4 py-2 text-sm text-neutral-700 hover:bg-neutral-100 hover:text-neutral-900">
                Realizacje
              </a>
            </div>
          </Transition>
        </div>

        <a href="#about" class="whitespace-nowrap text-sm font-normal text-neutral-900 hover:text-neutral-600">
          O firmie
        </a>

        <a href="#realisations" class="whitespace-nowrap text-sm font-normal text-neutral-900 hover:text-neutral-600">
          Realizacje
        </a>

        <a href="#contact" class="whitespace-nowrap text-sm font-normal text-neutral-900 hover:text-neutral-600">
          Kontakt
        </a>

        <!-- Szalony Search Bar -->
        <div
            ref="searchContainerRef"
            class="relative flex items-center justify-end rounded-full transition-all duration-[800ms] ease-[cubic-bezier(0.68,-0.55,0.26,1.55)]"
            :class="isSearchExpanded ? 'w-64 bg-gray-900 border border-fuchsia-500 shadow-xl' : 'w-10 bg-transparent'"
        >
          <div
              class="absolute -inset-1 bg-gradient-to-r from-fuchsia-600 via-purple-600 to-pink-600 rounded-full blur-md transition-opacity duration-700 -z-10"
              :class="isSearchExpanded ? 'opacity-60 animate-pulse' : 'opacity-0'"
          ></div>

          <input
              ref="searchInput"
              v-model="searchQuery"
              type="text"
              placeholder="Szukaj..."
              class="bg-transparent text-white placeholder-fuchsia-300/50 outline-none transition-all duration-[800ms] ease-[cubic-bezier(0.68,-0.55,0.26,1.55)]"
              :class="isSearchExpanded ? 'w-full opacity-100 pl-4 pr-2 translate-x-0' : 'w-0 opacity-0 translate-x-12 pointer-events-none'"
              @keydown.enter="handleSearch"
              @keydown.esc="closeSearch"
          />

          <button
              @click.stop="toggleSearch"
              aria-label="Szukaj"
              class="z-10 flex h-10 w-10 flex-shrink-0 items-center justify-center rounded-full transition-all duration-[800ms] ease-[cubic-bezier(0.68,-0.55,0.26,1.55)]"
              :class="isSearchExpanded ? 'rotate-[360deg] text-fuchsia-400 bg-gray-800 scale-90 shadow-[0_0_15px_rgba(217,70,239,0.5)]' : 'text-neutral-900 hover:text-neutral-600 hover:scale-110 hover:-rotate-12'"
          >
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
          </button>
        </div>
      </div>

      <!-- MOBILE: Przycisk Hamburgera -->
      <button
          @click="toggleMobileMenu"
          aria-label="Menu mobilne"
          class="relative z-50 flex h-10 w-10 flex-col items-center justify-center md:hidden focus:outline-none"
      >
        <!-- Górna kreska -->
        <span
            class="absolute h-0.5 w-6 bg-neutral-900 transition-all duration-300 ease-in-out"
            :class="isMobileMenuOpen ? 'rotate-45 translate-y-0' : '-translate-y-2'"
        ></span>
        <!-- Środkowa kreska -->
        <span
            class="absolute h-0.5 w-6 bg-neutral-900 transition-all duration-300 ease-in-out"
            :class="isMobileMenuOpen ? 'opacity-0 scale-x-0' : 'opacity-100'"
        ></span>
        <!-- Dolna kreska -->
        <span
            class="absolute h-0.5 w-6 bg-neutral-900 transition-all duration-300 ease-in-out"
            :class="isMobileMenuOpen ? '-rotate-45 translate-y-0' : 'translate-y-2'"
        ></span>
      </button>
    </div>

    <!-- MOBILE: Overlay z menu -->
    <!-- MOBILE: Overlay z menu -->
    <Transition
        enter-active-class="transition duration-300 ease-out"
        enter-from-class="opacity-0 -translate-y-full"
        enter-to-class="opacity-100 translate-y-0"
        leave-active-class="transition duration-200 ease-in"
        leave-from-class="opacity-100 translate-y-0"
        leave-to-class="opacity-0 -translate-y-full"
    >
      <div
          v-if="isMobileMenuOpen"
          class="fixed top-[72px] left-0 w-full h-[calc(100vh-72px)] z-40 bg-white flex flex-col px-6 py-8 overflow-y-auto md:hidden shadow-xl"
      >
        <div class="flex flex-col gap-6 text-xl font-medium">

          <!-- Rozwijana Oferta w Mobile -->
          <div class="flex flex-col">
            <button
                @click="isMobileOfferOpen = !isMobileOfferOpen"
                class="flex items-center justify-between py-2 text-neutral-900"
            >
              <span>Oferta</span>
              <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-5 w-5 transition-transform duration-200"
                  :class="{ 'rotate-180': isMobileOfferOpen }"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  stroke-width="2"
              >
                <path stroke-linecap="round" stroke-linejoin="round" d="M19 9l-7 7-7-7" />
              </svg>
            </button>

            <div
                v-if="isMobileOfferOpen"
                class="flex flex-col pl-4 gap-3 mt-2 border-l-2 border-gray-100 text-lg text-neutral-600"
            >
              <a href="#offer" @click="closeMobileMenu" class="py-1">Projekty</a>
              <a href="#offer" @click="closeMobileMenu" class="py-1">Wizualizacje</a>
              <a href="#offer" @click="closeMobileMenu" class="py-1">Realizacje</a>
            </div>
          </div>

          <a href="#about" @click="closeMobileMenu" class="py-2 text-neutral-900 border-b border-gray-100">
            O firmie
          </a>
          <a href="#realisations" @click="closeMobileMenu" class="py-2 text-neutral-900 border-b border-gray-100">
            Realizacje
          </a>
          <a href="#contact" @click="closeMobileMenu" class="py-2 text-neutral-900 border-b border-gray-100">
            Kontakt
          </a>
        </div>

        <!-- Dodatkowy element na dole (wyszukiwanie) -->
        <div class="mt-auto pt-8 pb-12">
          <div class="relative flex items-center w-full bg-gray-50 border border-gray-200 rounded-xl px-4 py-3">
            <input
                v-model="searchQuery"
                type="text"
                placeholder="Szukaj na stronie..."
                class="w-full bg-transparent text-neutral-900 placeholder-neutral-400 outline-none text-sm"
                @keydown.enter="handleSearch(); closeMobileMenu();"
            />
            <button @click="handleSearch(); closeMobileMenu();" class="text-neutral-600">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                <circle cx="11" cy="11" r="8"></circle>
                <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
              </svg>
            </button>
          </div>
        </div>
      </div>
    </Transition>
  </nav>
</template>

<style scoped>
</style>