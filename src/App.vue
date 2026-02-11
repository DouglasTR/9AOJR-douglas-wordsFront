<template>
  <div id="app">
    <Header logoText="Douglas Words Front" />
    <router-view />
    <Footer />
  </div>
</template>

<script setup>
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'
</script>
