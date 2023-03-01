<script setup lang="ts">
useHead({
  meta: [
    { property: 'og:title', content: 'Chuanqi Jiao' },
    { name: 'description', content: 'Chuanqi Jiao\'s Portfolio' },
  ],
})
</script>

<template>
  <NavBar />
  <main class="px-7 py-10">
    <router-view />
    <Footer />
  </main>
</template>
