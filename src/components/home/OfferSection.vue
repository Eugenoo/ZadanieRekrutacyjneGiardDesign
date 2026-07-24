<script setup lang="ts">
import OfferCard from './OfferCard.vue'

const offers = [
  {
    id: 1,
    title: 'Projekty',
    text: 'Zaprojektujemy Twój ogród w nowoczesnym stylu i z najlepszym wykorzystaniem istniejącej przestrzeni.',
    linkText: 'Dowiedz się więcej',
    href: '#realisations',
    icon: '../../../../src/assets/icons/Icon-pen.png'
  },
  {
    id: 2,
    title: 'Wizualizacje',
    text: 'Przedstawimy Ci projekty koncepcyjne w postaci wirtualnego spaceru animowanego w technologii 3D.',
    linkText: 'Dowiedz się więcej',
    href: '#realisations',
    icon: '../../../../src/assets/icons/Icon-visualistaions.png'
  },
  {
    id: 3,
    title: 'Realizacje',
    text: 'Zrealizujemy Twoje marzenie przy użyciu najnowszych rozwiązań i zaawansowanych technologii.',
    linkText: 'Zobacz nasze realizacje',
    href: '#realisations',
    icon: '../../../../src/assets/icons/Icon-realisations.png'
  }
]
</script>

<template>
  <section class="w-full bg-stone-custom  py-12 lg:py-16 flex flex-col justify-between items-center overflow-hidden">

    <div class="w-full max-w-[1040px] px-6 py-6 mx-auto flex flex-col gap-6">
      <p class="text-xs font-semibold uppercase tracking-wider text-green-900 font-['Inter']">
        Oferta
      </p>

      <div class="max-w-[709px] flex flex-col gap-8">
        <h2 class="text-neutral-900 text-5xl font-medium leading-[55px] font-['Montserrat']">
          <span class="font-heading">Działamy </span>
          <span class="italic">kompleksowo</span>
        </h2>

        <p class="text-neutral-900 text-base leading-6 font-['Inter']">
          Oferujemy kompletną obsługę inwestycji terenów zielonych.
          Projektujemy nowoczesne ogrody przydomowe oraz rezydencjonalne. Stworzymy dla Ciebie projekt, zwizualizujemy go i wcielimy w życie, a na każdym etapie posłużymy radą i wieloletnim doświadczeniem.
        </p>
      </div>
    </div>

    <!-- 2. KARTY: Marginesy boczne px-[88px] oraz gap-16 (64px) -->
    <div class="w-full max-w-[1440px] mx-auto px-4 sm:px-16 md:px-[88px] flex-1 flex items-center">
      <div class="grid grid-cols-1 lg:grid-cols-3 gap-8 md:gap-[64px] w-full justify-items-center">
        <OfferCard class="max-w-[420px]"
            v-for="offer in offers"
            :key="offer.id"
            :title="offer.title"
            :text="offer.text"
            :link-text="offer.linkText"
            :href="offer.href"
            :icon="offer.icon"
        />
      </div>
    </div>

  </section>
</template>