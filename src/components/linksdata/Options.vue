<template>
<div class="text-primary mx-auto max-w-screen-md px-4 py-8 sm:px-6 lg:px-8">

    <div class="text-sm filter-options grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4">
        <div v-for="option in options" :key="option.id" class="">
            <input type="checkbox" :id="option.id" :value="option.id" v-model="selectedOptions" class="mr-2" />
            <label :for="option.id" class="mr-4">{{ option.label }}</label>
        </div>
    </div>

    <SearchBarToogle v-model="searchTerm" :placeholder="searchPlaceholder" />

    <div class="tools-info flex flex-col md:flex-row justify-between items-start">
        <span class="mb-2 md:mb-0"> Showing {{ displayedItems.length }} of {{ items.length }} Total Tools</span>
        <div class="relative">
            <div class="inline-flex items-center overflow-hidden rounded-md border bg-white">
                <a href="#" class="border-e px-4 py-2 text-sm/none text-gray-600 hover:bg-gray-50 hover:text-gray-700">
                    Sort by: {{ selectedSort }}
                </a>

                <button @click="dropdownOpen = !dropdownOpen" class="h-full p-2 text-gray-600 hover:bg-gray-50 hover:text-gray-700">
                    <span class="sr-only">Menu</span>
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" viewBox="0 0 20 20" fill="currentColor">
                        <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
                    </svg>
                </button>
            </div>

            <div v-if="dropdownOpen" class="absolute end-0 z-10 mt-2 w-56 rounded-md border border-gray-100 bg-white shadow-lg">
                <li v-for="sortOption in sortOptions" :key="sortOption.value" class="transition-all">
                    <a class="block cursor-pointer rounded-lg px-4 py-2 text-sm text-gray-500 hover:bg-gray-50 hover:text-gray-700" @click="selectSort(sortOption.value)">{{ sortOption.label }}</a>
                </li>
            </div>
        </div>
    </div>

    <div class="text-primary grid grid-cols-1 md:grid-cols-2 gap-4 mt-6">
        <article v-for="item in displayedItems" :key="item.id" class="rounded-lg border-2 border-gray-100">
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
                </div>
            </a>
        </article>

        <div class="txt-preimary flex items-center justify-start py-4 gap-3">
            <a @click.prevent="goToPreviousPage" class="cursor-pointer inline-flex h-8 w-8 items-center justify-center rounded border border-gray-100 bg-white  rtl:rotate-180">
                <span class="sr-only">Previous Page</span>
                <svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3" viewBox="0 0 20 20" fill="currentColor">
                    <path fill-rule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clip-rule="evenodd" />
                </svg>
            </a>

            <p class="text-xs ">
                {{ currentPage }} <span class="mx-0.25">/</span> {{ totalPages }}
            </p>

            <a @click.prevent="goToNextPage" class="cursor-pointer inline-flex h-8 w-8 items-center justify-center rounded border border-gray-100 bg-white  rtl:rotate-180">
                <span class="sr-only">Next Page</span>
                <svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3" viewBox="0 0 20 20" fill="currentColor">
                    <path fill-rule="evenodd" d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z" clip-rule="evenodd" />
                </svg>
            </a>
        </div>
    </div>
</div>
</template>

<script setup>
import {
    ref,
    computed
} from 'vue';
import SearchBarToogle from '../../components/search/SearchToggle.vue'
import itemData from '../../data/items.json'
import OptionData from '../../data/options.json'
import SortOptionData from '../../data/sortOptions.json'

const items = ref(itemData);
const options = OptionData;
const sortOptions = SortOptionData;

const itemsPerPage = 10; // Number of items to display per page

const searchPlaceholder = ref('Search..')
const searchTerm = ref('');
const selectedSort = ref('default');
const dropdownOpen = ref(false);
const selectedOptions = ref([]);
const currentPage = ref(1);

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

      // Pagination logic
    const startIndex = (currentPage.value - 1) * itemsPerPage;
    const endIndex = startIndex + itemsPerPage;
    return sortedItems.slice(startIndex, endIndex);
});

const displayedItemsLength = computed(() => displayedItems.value.length);

const totalPages = computed(() =>
  Math.ceil(displayedItemsLength.length / itemsPerPage)
);

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

const searchItems = () => {
    // Implement the search logic when the input value changes
    // The computed property 'displayedItems' will automatically update
};

const clearSearch = () => {
    searchTerm.value = '';
};

const goToPreviousPage = () => {
  if (currentPage.value > 1) {
    currentPage.value--;
  }
};

const goToNextPage = () => {
  if (currentPage.value < totalPages.value) {
    currentPage.value++;
  }
};
</script>