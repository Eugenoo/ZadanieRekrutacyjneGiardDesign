<script setup>

let fullYear = new Date().getFullYear()
</script>

<template>
  <footer class="bg-neutral-900 px-6 py-12 md:px-12 lg:px-0 lg:py-20 flex justify-center">

    <div class="w-full max-w-[1040px] flex flex-col gap-12 md:gap-28">

      <div class="flex flex-col gap-10 md:gap-16">

        <div class="flex flex-col md:flex-row justify-between items-start md:items-center gap-8 md:gap-0">

          <img
              src="../../assets/giarddesign.svg"
              alt="GiardDesign"
              class="h-6 md:h-auto"
          >

          <div class="flex flex-col sm:flex-row items-start sm:items-center gap-4 md:gap-6 w-full md:w-auto">

            <p class="text-stone-100 text-sm md:text-base">
              Daj znać, co możemy dla Ciebie zrobić!
            </p>

            <a
                href="#"
                class="px-6 py-3.5 bg-green-900 rounded-full text-stone-100 text-center text-sm md:text-base w-full sm:w-auto transition-colors hover:bg-green-800"
            >
              Skontaktuj się z nami
            </a>

          </div>

        </div>

        <div class="border-t border-stone-100 opacity-30 md:opacity-100"></div>

        <div class="flex flex-col md:flex-row justify-between gap-10 md:gap-0">

          <nav class="flex flex-col sm:flex-row gap-4 sm:gap-6 lg:gap-12">
            <a href="#" class="text-stone-100 text-sm">Kontakt</a>
            <a href="#" class="text-stone-100 text-sm">Instagram</a>
            <a href="#" class="text-stone-100 text-sm">Facebook</a>
            <a href="#" class="text-stone-100 text-sm">LinkedIn</a>
          </nav>

          <div class="flex flex-col sm:flex-row gap-4 sm:gap-6 lg:gap-12">
            <span class="text-stone-100 text-sm">000-000-000</span>
            <span class="text-stone-100 text-sm">giarddesign@kontakt.pl</span>
          </div>

        </div>

      </div>

      <div class="flex flex-col md:flex-row justify-between items-start md:items-center gap-6 md:gap-0">

        <p class="text-stone-100 text-sm md:text-base">
          Prawa zastrzeżone © {{fullYear}}
        </p>

        <div class="flex items-center gap-4">

        <span class="text-stone-100 text-sm md:text-base">
          made by
        </span>
          <a href="https://adrespect.pl/">
            <img
                src="../../assets/logoad.svg"
                alt="Adapter"
                class="w-24 md:w-28"
            />
          </a>
        </div>

      </div>

    </div>

  </footer></template>