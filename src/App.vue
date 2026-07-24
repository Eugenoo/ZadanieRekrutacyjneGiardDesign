<script setup>
import Navbar from "@/components/layout/navbar.vue";
import HeroSlide from "@/components/home/HeroSlide.vue";
import OfferSection from "@/components/home/OfferSection.vue";
import AboutSection from "@/components/home/AboutSection.vue";
import ProjectSection from "@/components/home/ProjectSection.vue";
import InstagramSection from "@/components/home/InstagramSection.vue";
import Footer from "@/components/layout/footer.vue";
</script>

<template>
  <navbar></navbar>
  <HeroSlide></HeroSlide>
  <OfferSection id="offer"></OfferSection>
  <AboutSection id="about"></AboutSection>
  <ProjectSection id="realisations"></ProjectSection>
  <InstagramSection id="contact"></InstagramSection>
  <Footer></Footer>
</template>

<style scoped></style>
