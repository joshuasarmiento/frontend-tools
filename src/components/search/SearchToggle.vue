<template>
    <section>
        <div class="text-primary flex items-center justify-center gap-4 pb-4">
            <div class="flex items-center gap-4">
                <div class="relative">
                    <label class="sr-only" for="search"> Search </label>
                    <input class="h-10 w-full shadow-md  rounded-full border-none pe-10 ps-4 text-sm sm:w-56" id="search" type="search" placeholder="Search website..." />
                    <button type="button" class="absolute end-1 top-1/2 -translate-y-1/2 rounded-full p-2  transition ">
                        <span class="sr-only">Search</span>
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
                        </svg>
                    </button>
                </div>
            </div>

            <span aria-hidden="true" class="block h-6 w-px rounded-full bg-gray-300"></span>

            <a @click="!toggleDark(); toggleTheme();" class="cursor-pointer block shrink-0 rounded-full border border-white shadow-md  p-2.5 text-gray-600 hover:text-gray-700">
                <span class="sr-only">IsDarkmode</span>
                <span>
                    <img :src="icon" class="w-5 h-5 text-neutral-100" :alt="iconAlt" />
                </span>
            </a>
        </div>
    </section>
</template>

<script setup>
import {
    ref,
    computed,
} from "vue";
import MoonLight from "../../assets/svg/moon-light.svg";
import SunLight from "../../assets/svg/sun-light.svg";

import {
    useDark,
    useToggle
} from "@vueuse/core";

const iconBlack = MoonLight;
const iconWhite = SunLight;

const isDarkmode = ref(false);
const icon = computed(() => (isDarkmode.value ? iconWhite : iconBlack ));
const iconAlt = computed(() => (isDarkmode.value ? "Sun Light" : "Moon Light"));

const isDark = useDark();
console.log(isDark.value);

const toggleDark = useToggle(isDark);
const toggleTheme = () => {
    isDarkmode.value = !isDarkmode.value;
};
</script>

<style>
.dark {
  @apply bg-gradient-to-br from-neutral-950 via-zinc-950 to-stone-950;
}
</style>
