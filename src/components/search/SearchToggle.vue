<template>
<div class="flex items-center justify-center gap-4">
    <div class="flex items-center gap-4">
        <div class="relative">
            <label class="sr-only" for="search"> Search </label>

            <input class="h-10 w-full rounded-full border-none bg-white pe-10 ps-4 text-sm shadow-sm sm:w-56" id="search" type="search" placeholder="Search website..." />

            <button type="button" class="absolute end-1 top-1/2 -translate-y-1/2 rounded-full bg-gray-50 p-2 text-gray-600 transition hover:text-gray-700">
                <span class="sr-only">Search</span>
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
                </svg>
            </button>
        </div>

        <a href="#" class="block shrink-0 rounded-full bg-white p-2.5 text-gray-600 shadow-sm hover:text-gray-700">
            <span class="sr-only">Notifications</span>
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                <path stroke-linecap="round" stroke-linejoin="round" d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9" />
            </svg>
        </a>
    </div>

    <span aria-hidden="true" class="block h-6 w-px rounded-full bg-gray-300"></span>

    <a @click="!toggleDark(); toggleTheme();" class="cursor-pointer block shrink-0 rounded-full bg-white dark:bg-neutral-600 p-2.5 text-gray-600 shadow-sm hover:text-gray-700">
        <span class="sr-only">IsDarkmode</span>
        <span  >
            <img :src="icon" class="w-5 h-5 text-neutral-100" :alt="iconAlt" />
        </span>
    </a>

</div>
</template>

<script setup>
import {
    ref,
    computed,
} from "vue";
import MoonLight from "../../assets/svg/moon-light.svg";
import SunLight from "../../assets/svg/sun-light.svg";
import myLogoWhite from '../../assets/svg/mylogo-white.svg'
import myLogoBlack from '../../assets/svg/mylogo-black.svg'

import {
    useDark,
    useToggle
} from "@vueuse/core";

const logoWhite = myLogoWhite;
const logoBlack = myLogoBlack;
const iconBlack = MoonLight;
const iconWhite = SunLight;

const isDarkmode = ref(true);
const icon = computed(() => (!isDarkmode.value ? iconWhite : iconBlack));
const logo = computed(() => (isDarkmode.value ? logoWhite : logoBlack));
const iconAlt = computed(() => (isDarkmode.value ? "Moon Light" : "Sun Light"));

const isDark = useDark();
console.log(isDark.value);

const toggleDark = useToggle(isDark);
const toggleTheme = () => {
    isDarkmode.value = !isDarkmode.value;
};
</script>

<style>

</style>
