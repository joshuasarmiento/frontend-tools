<template>
<div class="text-primary">
    <div class="gap-4">
        <div class="relative">
            <label class="sr-only" for="search"> Search </label>
            <input class="h-9 w-full border-neutral-400 pe-10 ps-4 text-sm dark:text-neutral-800" id="search" v-model="searchTerm" @input="searchItems" type="text" :placeholder="placeholder" />
            <button type="button" class="absolute end-1 top-1/2 -translate-y-1/2 rounded-full p-1 transition-all ">
                <span class="sr-only">Search</span>
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 dark:text-neutral-800" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
                </svg>
            </button>
        </div>
    </div>
</div>
</template>

<script setup>
import {
    ref,
    watch,
    defineProps,
    defineEmits
} from "vue";
const props = defineProps({
    modelValue: {
        type: String,
        default: '',
    },
    placeholder: {
        type: String,
        default: 'Search...',
    },
})
const searchTerm = ref(props.modelValue);
watch(
    () => props.modelValue,
    (newValue) => {
        searchTerm.value = newValue;
    }
);
const emit = defineEmits(['update:modelValue', 'clear']);
const searchItems = () => {
    emit('update:modelValue', searchTerm.value);
};
const clearSearch = () => {
    searchTerm.value = '';
    emit('clear');
};
</script>
