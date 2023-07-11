<template>
     <div>
        <div class="flex mb-4 items-center justify-center text-primary">
            <button v-for="(tab, index) in tabs" :key="index" :class="{
            'p-2 shadow-md bg-gray-100 border dark:text-neutral-600': activeTab === tab,
            'p-2 shadow-md border border-neutral-100 dark:border-neutral-100': activeTab !== tab,
            'rounded-tl-lg': index === 0,
            'rounded-tr-lg': index === tabs.length - 1,
            'rounded-bl-lg': index === 0,
            'rounded-br-lg': index === tabs.length - 1,
            }" @click="activeTab = tab">
                {{ tab }}
            </button>
        </div>
        <div class="p-4 rounded-lg text-primary">
            <template v-if="activeTab === 'Home'">
                <OptionsVue />
            </template>
            <template v-if="activeTab === 'Glossary'">
                <div class="flex flex-col items-center">
                    <h2 class="text-2xl font-bold mb-2">Glossary</h2>
                    <p>Coming soon..</p>
                </div>
            </template>
            <template v-if="activeTab === 'Submit'">
                <div class="flex flex-col items-center">
                    <h2 class="text-2xl font-bold mb-2">Submit</h2>
                    <p>Coming soon..</p>
                </div>
            </template>
        </div>
    </div>
</template>

<script setup>
import OptionsVue from '../components/linksdata/Options.vue'

import {
    ref
} from 'vue';

const tabs = ['Home', 'Glossary', 'Submit'];
const activeTab = ref('Home');
</script>
