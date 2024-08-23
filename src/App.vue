<script setup>
import Map from './components/Map.vue'
import Results from './components/Results.vue'
import data from './assets/data.json'
import { ref } from 'vue';

const selectedCity = ref("")
const hoveredCity = ref({})

function cityClicked(city) {
  selectedCity.value = city
}

function cityHovered(city) {
  hoveredCity.value = city;
}
</script>

<template>
  <header class="m-8">
    <h1 className="text-4xl font-extrabold leading-none">
      Urfehdenkarte
    </h1>
  </header>

  <main class="m-8 space-x-6 flex justify-around">
    <div id="mapcontainer">
      <Map @cityclicked="cityClicked" img_src="/map.gif" :data="data" :hoveredCity="hoveredCity"/>
    </div>
    <div id="rescontainer" class="flex-1">
      <Results @cityhover="cityHovered" :data="data" :selectedCity="selectedCity" />
    </div>
  </main>
</template>

<style scoped>
main {
  max-height: 80vh;
  min-height: 80vh;
}

#mapcontainer {
  flex: 1;
}

#rescontainer {
  flex: 1;
}
</style>
