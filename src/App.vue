<template>
  <div id="app" class="flex flex-col min-h-screen">
    <Header logoText="Douglas Words Front" />
    <main class="flex-grow">
      <router-view />
    </main>
    <Footer />
  </div>
</template>

<script setup>
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'
</script>
