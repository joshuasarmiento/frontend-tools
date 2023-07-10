<template>
<main>
    <HeaderVue />
    <SearchToggleVue />
</main>
</template>

<script setup>
import HeaderVue from '../components/header/Header.vue'
import SearchToggleVue from '../components/search/SearchToggle.vue'
</script>

<style>
.dark {
  @apply bg-gradient-to-br from-neutral-950 via-zinc-950 to-stone-950;
}
</style>
