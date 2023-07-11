<template>
<div class="mx-4 md:mx-10">
    <h2 class="text-2xl font-bold mb-4 text-primary">Add New Item</h2>
    <form @submit.prevent="submitForm" class="space-y-4">
        <div>
            <label for="name" class="block font-semibold">Your Name:</label>
            <input required v-model="formData.name" type="text" id="name" class="w-full h-9 dark:text-neutral-900">
        </div>
        <div>
            <label for="title" class="block font-semibold">Title:</label>
            <input required v-model="formData.title" type="text" id="title" class="w-full h-9 dark:text-neutral-900">
        </div>
        <div>
            <label for="content" class="block font-semibold">Content:</label>
            <textarea required v-model="formData.content" id="content" class="w-full h-21 dark:text-neutral-900"></textarea>
        </div>
        <div>
            <label for="link" class="block font-semibold">Link:</label>
            <input required v-model="formData.link" type="text" id="link" class="w-full h-9 dark:text-neutral-900">
        </div>
        <div>
            <label for="dateAdded" class="block font-semibold">Date Added:</label>
            <input required disabled v-model="formData.dateAdded" type="text" id="dateAdded" class="w-full h-9 dark:text-neutral-900" :placeholder="formatted">
        </div>
        <div>
            <label class="block font-semibold">Options:</label>
            <div class="grid grid-cols-2 lg:grid-cols-3 gap-2 mt-2">
                <label v-for="option in options" :key="option" class="flex items-center">
                    <input type="checkbox" v-model="formData.options" :value="option.id">
                    <span class="ml-2">{{ option.label }}</span>
                </label>
            </div>
        </div>
        <button type="submit" class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2">Submit</button>
    </form>

    <section v-if="toggle" class="fixed top-10 right-5 animate-fade-left">
        <div role="alert" class="border border-gray-100 bg-white p-4 shadow-xl">
            <div class="flex items-start gap-4">
                <span class="text-green-600">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="h-6 w-6">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75L11.25 15 15 9.75M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                    </svg>
                </span>

                <div class="flex-1">
                    <strong class="block font-medium text-gray-900"> Changes saved </strong>

                    <p class="mt-1 text-sm text-gray-700">
                        Your product changes have been saved.
                    </p>
                </div>

                <button @click="toggle = false" class="text-gray-500 transition hover:text-gray-600">
                    <span class="sr-only">Dismiss popup</span>

                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="h-6 w-6">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
                    </svg>
                </button>
            </div>
        </div>
    </section>
</div>
</template>

<script setup>
import {
    useNow,
    useDateFormat
} from '@vueuse/core'

import {
    ref
} from 'vue';
import OptionData from '../data/options.json'

const toggle = ref(false)
const formatted = useDateFormat(useNow(), 'DD-MM-YYYY', {
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
    }, 5000);

    // if (formData.value.content === '') {
    //     // Handle the case when content is empty
    //     return;
    // }

    // axios.post('https://project-apis.onrender.com/frontendTools', formData.value)
    //     .then(response => {
    //         // Handle the successful response
    //         console.log(response.data);
    //     })
    //     .catch(error => {
    //         // Handle the error
    //         console.error(error);
    //     });
};
</script>
