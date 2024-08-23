<script setup>
import { ref, watch } from 'vue';

const props = defineProps({
    data: {
        type: Array,
        required: true
    },
    selectedCity: {
        type: String,
        required: true
    }
})

const numResults = ref(0)

const currentSearch = ref("")

watch(() => props.selectedCity, (newcity, oldcity) => {
    currentSearch.value = newcity
});

function filterArray() {
    const res = props.data.filter((entry) => {
        return entry.city.toLowerCase().includes(currentSearch.value.toLowerCase())
    })
    numResults.value = res.length;

    let scrollArea = document.getElementById("results");
    if (scrollArea)
        scrollArea.scrollTop = 0;

    return res;
}
</script>

<template>
    <div id="result-container" class="flex flex-col">
        <div class="flex mb-6 items-center">
            <input autocomplete="off" id="search" type="text"
                class="mr-6 bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                placeholder="Stadt suchen" v-model="currentSearch">
            <button @click="currentSearch = ''" id="clear" type="button"
                class="h-full mb-0 text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800">
                Löschen
            </button>
        </div>
        <div id="results">
            <div v-for="entry in filterArray()" class="mb-6 rounded overflow-hidden shadow-xl"
                @mouseenter="$emit('cityhover', entry)" @click="selectedCity = entry.city">
                <div class="px-6 py-4">
                    <div class="flex justify-between items-center">
                        <div class="font-bold text-xl mb-2">{{ entry.name }}</div>
                        <div class="text-sm">{{ entry.date }}</div>
                    </div>
                    <div class="text-sm mb-3 text-gray-500">{{ entry.code }}</div>
                    <p class="text-gray-700 text-base">
                        {{ entry.description }}
                    </p>
                </div>
                <div class="px-6 pt-4 pb-2">
                    <span
                        class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">{{
                            entry.city }}</span>
                </div>
            </div>
        </div>
        <div class="mt-8 text-right italic">{{ numResults }} <span v-if="numResults !== 1">Ergebnisse</span><span
                v-else>Ergebnis</span></div>
    </div>
</template>

<style scoped>
#result-container {
    height: 100%;
}

#results {
    height: 100%;
    overflow-y: scroll;
}
</style>