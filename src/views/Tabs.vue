<template>
     <div class="">
        <div class="flex mb-4 mx-4 md:mx-10 items-center justify-center text-primary">
            <button v-for="(tab, index) in tabs" :key="index" :class="{
            'p-1 border border-neutral-400 bg-neutral-100 dark:text-neutral-600 w-1/2 ': activeTab === tab,
            'p-1 border border-neutral-400 dark:border-neutral-100 w-2/3': activeTab !== tab
            }" @click="activeTab = tab">
                {{ tab }}
            </button>
        </div>
        <div class="text-primary animate-fade-down">
            <template v-if="activeTab === 'Home'">
                <OptionsVue />
            </template>
            <template v-if="activeTab === 'Glossary'">
                <GlossaryVue />
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
import GlossaryVue from './Glossary.vue'

import {
    ref
} from 'vue';

const tabs = ['Home', 'Glossary', 'Submit'];
const activeTab = ref('Home');
</script>
