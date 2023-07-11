<template>
    <div class="flex justify-center p-4">
        <a @click="!toggleDark(); toggleTheme();" class="cursor-pointer hover:animate-wiggle-more">
            <span class="sr-only">IsDarkmode</span>
            <span>
                <!-- <img :src="icon" class="w-5 h-5 text-neutral-100" :alt="iconAlt" /> -->
                <span v-if="isDarkmode">🌑</span>
                <span v-else>🌞</span>
            </span>
        </a>
    </div>
</template>

<script setup>
import {
    ref,
    computed
} from 'vue';
import MoonLight from "../../assets/svg/moon-light.svg";
import SunLight from "../../assets/svg/sun-light.svg";

import {
    useDark,
    useToggle
} from "@vueuse/core";


const iconBlack = MoonLight;
const iconWhite = SunLight;
// 🌙 🌑 🌚
// ☀️ 🌞
// ⭐
const isDarkmode = ref(true);
const icon = computed(() => (isDarkmode.value ? iconBlack : iconWhite));
const iconAlt = computed(() => (isDarkmode.value ?"Moon Light" : "Sun Light"));

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