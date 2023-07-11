<template>
     <div class="">
        <div class="flex mb-4 mx-4 md:mx-10 items-center justify-center text-primary">
            <button v-for="(tab, index) in tabs" :key="index" :class="{
            'p-1 shadow-md bg-gray-100 border dark:text-neutral-600 w-1/2': activeTab === tab,
            'p-1 shadow-md border border-neutral-100 dark:border-neutral-100 w-2/3': activeTab !== tab
            }" @click="activeTab = tab">
                {{ tab }}
            </button>
        </div>
        <div class="text-primary">
            <template v-if="activeTab === 'Home'">
                <OptionsVue />
            </template>
            <template v-if="activeTab === 'Glossary'">
                <div class="flex flex-col items-center">
                    <p>Coming soon..</p>
                </div>
            </template>
            <template v-if="activeTab === 'Submit'">
                <AddData />
            </template>
        </div>
    </div>
</template>

<script setup>
import OptionsVue from '../components/linksdata/Options.vue'
import AddData from './AddData.vue'

import {
    ref
} from 'vue';

const tabs = ['Home', 'Glossary', 'Submit'];
const activeTab = ref('Home');
</script>
