<template>
<div class="text-primary mx-4 md:mx-10">
    <form @submit.prevent="submitForm" class="space-y-4">
        <div class="flex flex-col md:flex-row items-center space-y-2 md:space-x-2 md:space-y-0">
            <div class="w-full">
                <label for="name" class="block text-sm mb-2">Your Name:</label>
                <input required v-model="formData.name" type="text" id="name" class="w-full h-8 dark:text-neutral-900">
            </div>
            <div class="w-full"> 
                <label for="title" class="block text-sm mb-2">Item Title:</label>
                <input required v-model="formData.title" type="text" id="title" class="w-full h-8 dark:text-neutral-900">
            </div>
        </div>
        <div class="flex flex-col md:flex-row items-center space-y-2 md:space-x-2 md:space-y-0">
            <div class="w-full">
                <label for="link" class="block text-sm mb-2">Item Link:</label>
                <input required v-model="formData.link" type="text" id="link" class="w-full h-8 dark:text-neutral-900">
            </div>
            <div class="w-full">
                <label for="dateAdded" class="block text-sm mb-2">Date Added:</label>
                <input required disabled v-model="formData.dateAdded" type="text" id="dateAdded" class="w-full h-8 bg-neutral-300 dark:text-neutral-600" :placeholder="formatted">
            </div>
        </div>
        <div>
            <label for="content" class="block text-sm mb-2">Item Description:</label>
            <textarea required v-model="formData.content" id="content" class="w-full h-18 dark:text-neutral-900"></textarea>
        </div>

        <div>
            <label class="block text-sm mb-2">Options:</label>
            <div class="grid grid-cols-2 md:grid-cols-3 mt-2">
                <label v-for="option in options" :key="option" class="flex items-center">
                    <input type="checkbox" v-model="formData.options" :value="option.id">
                    <span class="ml-2 text-sm">{{ option.label }}</span>
                </label>
            </div>
        </div>
        <button type="submit" class="border border-neutral-200 px-6 py-1 hover:bg-neutral-200 hover:text-neutral-900">Submit</button>
    </form>

    <section v-if="toggle" class="fixed right-0 bottom-0 animate-fade-left">
        <div role="alert" class="border border-gray-100 bg-white p-4 shadow-xl">
            <div class="flex items-start gap-4">
                <span class="text-green-600">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="h-6 w-6">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75L11.25 15 15 9.75M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                    </svg>
                </span>

                <div class="flex-1">
                    <strong class="block font-medium text-gray-900"> Item saved! </strong>

                    <p class="mt-1 text-sm text-gray-700">
                        Item have been saved.
                    </p>
                </div>

                <!-- <button @click="toggle = false" class="text-gray-500 transition hover:text-gray-600">
                    <span class="sr-only">Dismiss popup</span>

                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="h-6 w-6">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
                    </svg>
                </button> -->
            </div>
        </div>
    </section>
</div>
</template>

<script setup>
import axios from 'axios';
import {
    useNow,
    useDateFormat
} from '@vueuse/core'

import {
    ref
} from 'vue';
import OptionData from '../data/options.json'

const toggle = ref(false)
const formatted = useDateFormat(useNow(), 'YYYY-MM-DD', {
    locales: 'en-PH'
})

const formData = ref({
    name: '',
    title: '',
    content: '',
    link: '',
    dateAdded: formatted.value,
    options: []
});

const options = OptionData;

const submitForm = () => {
    // Perform form submission logic here
    // Scroll to top
    window.scrollTo({
        top: 0,
        left: 0,
        behavior: 'smooth'
    });

    console.log(formData.value);

    setTimeout(() => {
        toggle.value = true;
    }, 1000);
    setTimeout(() => {
        toggle.value = false;
        // Redirect to home
        window.location.href = '/frontend-tools/';
    }, 5000);

    if (formData.value.content === '') {
        // Handle the case when content is empty
        return;
    }

    axios.post(import.meta.env.VITE_BASE_URL, formData.value)
        .then(response => {
            // Handle the successful response
            console.log(response.data);
        })
        .catch(error => {
            // Handle the error
            console.error(error);
        });
};
</script>
