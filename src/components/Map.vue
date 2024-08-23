<script setup>
import { onMounted, ref, toRaw, watch } from 'vue';
import Dot from './Dot.vue'

import L from 'leaflet/dist/leaflet'

const props = defineProps({
  data: {
    type: Array,
    required: true
  },
  hoveredCity: {
    type: Object
  }
})

const emit = defineEmits(['cityclicked'])

let map = null

onMounted(() => {
  map = L.map('map').setView([48.7758, 9.1829], 8);
  L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 19,
    attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
  }).addTo(map);

  let uniqueCities = new Map()

  for (let entry of props.data) {
    if (!uniqueCities.has(entry.city)) {
      uniqueCities.set(entry.city, [entry.location.lat, entry.location.lng]);
    }
  }

  console.log("Before: " + props.data.length + ", after: " + uniqueCities.size)

  uniqueCities.forEach((location, city, m) => {
    const marker = L.marker(location).addTo(map);

    marker.on('click', () => {
      emit('cityclicked', city)
    })
  })

  // new ResizeObserver(() => {
  //   imgwidth.value = imgelement.value.clientWidth
  //   imgheight.value = imgelement.value.clientHeight
  // }).observe(imgelement.value)
})

watch(() => props.hoveredCity, async (newentry, oldentry) => {
  if (oldentry !== newentry) {
    var popup = L.popup()
      .setLatLng([newentry.location.lat, newentry.location.lng])
      .setContent(newentry.city)
      .openOn(map);
  }
})
</script>

<template>
  <div id="map"></div>
</template>

<style scoped>
#map {
  height: 100%;
}
</style>