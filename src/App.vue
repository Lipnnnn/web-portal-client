<template>
  <Navbar/>
  <router-view/>
</template>

<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Navbar from './components/Navbar.vue'
</script>

<style scoped>

</style>
