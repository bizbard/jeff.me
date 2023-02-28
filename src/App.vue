<script setup lang="ts">
useHead({
  meta: [
    { property: 'og:title', content: 'Chuanqi Jiao' },
    { property: 'og:image', content: 'https://jeff.me/tju.png' },
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
