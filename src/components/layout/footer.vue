<script setup>

let fullYear = new Date().getFullYear()
</script>

<template>
  <footer class="bg-neutral-900 px-64 py-20">

    <div class="w-[1040px] mx-auto flex flex-col gap-28">

      <!-- górna część -->
      <div class="flex flex-col gap-16">

        <div class="flex justify-between items-center">

          <!-- logo -->
          <img src="../../assets/giarddesign.svg">


          <div class="flex items-center gap-6">

            <p class="text-stone-100 text-base">
              Daj znać, co możemy dla Ciebie zrobić!
            </p>

            <a
                href="#"
                class="px-6 py-3.5 bg-green-900 rounded-full text-stone-100"
            >
              Skontaktuj się z nami
            </a>

          </div>

        </div>


        <div class="border-t border-stone-100"></div>


        <div class="flex justify-between">

          <nav class="flex gap-12">

            <a href="#" class="text-stone-100 text-sm">
              Kontakt
            </a>

            <a href="#" class="text-stone-100 text-sm">
              Instagram
            </a>

            <a href="#" class="text-stone-100 text-sm">
              Facebook
            </a>

            <a href="#" class="text-stone-100 text-sm">
              LinkedIn
            </a>

          </nav>


          <div class="flex gap-12">

            <span class="text-stone-100 text-sm">
              000-000-000
            </span>

            <span class="text-stone-100 text-sm">
              giarddesign@kontakt.pl
            </span>

          </div>

        </div>

      </div>


      <!-- copyright -->
      <div class="flex justify-between items-center">

        <p class="text-stone-100 text-base">
          Prawa zastrzeżone © {{fullYear}}
        </p>


        <div class="flex items-center gap-4">

          <span class="text-stone-100 text-base">
            made by
          </span>

          <img
              src="../../assets/logoad.svg"
              alt="Adapter"
              class="w-28"
          />

        </div>

      </div>

    </div>

  </footer>
</template>