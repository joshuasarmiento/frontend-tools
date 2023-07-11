<template>
<div class="text-primary mx-4 md:mx-10">

    <div class="text-sm filter-options grid grid-cols-2 md:grid-cols-3">
        <div v-for="option in options" :key="option.id" class="">
            <input type="checkbox" :id="option.id" :value="option.id" v-model="selectedOptions" class="mr-2" />
            <label :for="option.id" class="mr-4">{{ option.label }}</label>
        </div>
    </div>

    <SearchBarToogle v-model="searchTerm" :placeholder="searchPlaceholder" />

    <div class="tools-info flex flex-col md:flex-row justify-between items-center md:items-end">
        <span class="mb-2 md:mb-0"> Showing {{ displayedItems.length }} of {{ items.length }} Total Tools</span>
        <div class="relative">
            <div class="inline-flex items-center overflow-hidden border bg-white">
                <a href="#" class="border-e  px-4 py-2 text-sm/none text-gray-600 hover:bg-gray-50 hover:text-gray-700">
                    Sort by: {{ selectedSort }}
                </a>

                <button @click="dropdownOpen = !dropdownOpen" class="h-full p-2 text-gray-600 hover:bg-gray-50 hover:text-gray-700">
                    <span class="sr-only">Menu</span>
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" viewBox="0 0 20 20" fill="currentColor">
                        <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
                    </svg>
                </button>
            </div>

            <div v-if="dropdownOpen" class="absolute end-0 z-10 mt-2 p-4 w-56 border border-neutral-400 bg-white shadow-lg">
                <li v-for="sortOption in sortOptions" :key="sortOption.value" class="transition-all">
                    <a class="cursor-pointer text-xs text-neutral-600 hover:bg-gray-50 hover:text-gray-700" @click="selectSort(sortOption.value)">{{ sortOption.label }}</a>
                </li>
            </div>
        </div>
    </div>

    <div class="text-primary grid grid-cols-1 md:grid-cols-2 gap-4 mt-6">
        <span v-if="isLoading" class="text-sm">Loading...</span>
        <span v-else-if="displayedItems.length === 0 " class="text-sm"> No Data Found </span>
        <article v-else v-for="item in displayedItems" :key="item.id" class="border-2 border-neutral-400">
            <a :href="item.link" target="__blank" class="cursor-pointer flex items-start gap-4 p-4 sm:p-6 lg:p-8">
                <div>
                    <h3 class="font-medium sm:text-lg">
                        {{item.title}}
                    </h3>

                    <p class="line-clamp-2 text-sm text-secondary">
                        {{ item.content }}
                    </p>

                    <div class="mt-2 sm:flex sm:items-center sm:gap-2 text-secondary">
                        <div class="flex items-center gap-1">
                            <svg class="h-4 w-4" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                            </svg>
                            <p class="text-xs font-medium">{{ formatDate(item.dateAdded) }}</p>
                        </div>
                        <span class="hidden sm:block" aria-hidden="true">&middot;</span>
                        <p class="hidden sm:block sm:text-xs">
                            Upvotes:
                            <span class="font-medium ">
                                {{ item.upvotes}}
                            </span>
                        </p>
                    </div>
                    <div v-for="option in item.options" :key="option" class="mt-2 opacity-50">
                        <span class="text-xs p-1.5 border border-neutral-200">{{ option }}</span>
                    </div>
                </div>
            </a>
        </article>
    </div>
</div>
</template>

<script setup>
import axios from 'axios'

import {
    ref,
    computed,
    onMounted
} from 'vue';
import SearchBarToogle from '../../components/search/SearchToggle.vue'
// import itemData from '../../data/items.json'
import OptionData from '../../data/options.json'
import SortOptionData from '../../data/sortOptions.json'

const options = OptionData;
const sortOptions = SortOptionData;

const searchPlaceholder = ref('Search..')
const searchTerm = ref('');
const selectedSort = ref('default');
const dropdownOpen = ref(false);
const selectedOptions = ref([]);

const items = ref([]);
const isLoading = ref(false);
const displayedItems = computed(() => {
    let sortedItems = [...items.value];
    switch (selectedSort.value) {
        case 'upvotes':
            sortedItems.sort((a, b) => b.upvotes - a.upvotes);
            break;
        case 'nameAsc':
            sortedItems.sort((a, b) => a.title.localeCompare(b.title));
            break;
        case 'nameDesc':
            sortedItems.sort((a, b) => b.title.localeCompare(a.title));
            break;
        case 'dateDesc':
            sortedItems.sort((a, b) => new Date(b.dateAdded) - new Date(a.dateAdded));
            break;
        case 'dateAsc':
            sortedItems.sort((a, b) => new Date(a.dateAdded) - new Date(b.dateAdded));
            break;
            // 'default' case will display items in their original order
    }

    if (selectedOptions.value.length > 0) {
        sortedItems = sortedItems.filter((item) =>
            selectedOptions.value.some((option) => item.options.includes(option))
        );
    }

    // Apply search filtering if searchTerm has a value
    if (searchTerm.value) {
        sortedItems = sortedItems.filter((item) =>
            item.title.toLowerCase().includes(searchTerm.value.toLowerCase())
        );
    }

    return sortedItems;
});

const formatDate = (date) => {
    const options = {
        year: 'numeric',
        month: 'long',
        day: 'numeric'
    };
    return new Date(date).toLocaleDateString(undefined, options);
};

const selectSort = (sortValue) => {
    selectedSort.value = sortValue;
    dropdownOpen.value = false;
};

const fetchData = async () => {
    isLoading.value = true;
    try {
        const response = await axios.get(import.meta.env.VITE_BASE_URL);
        items.value = response.data;
    } catch (error) {
        console.error(error);
    } finally {
        isLoading.value = false;
    }
};

onMounted(fetchData);

const searchItems = () => {
    // Implement the search logic when the input value changes
    // The computed property 'displayedItems' will automatically update
};

const clearSearch = () => {
    searchTerm.value = '';
};
</script>