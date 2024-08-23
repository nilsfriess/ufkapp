<script setup>
import { computed, onMounted, onUpdated, ref, watch } from 'vue';

const props = defineProps({
    location: {
        type: Object,
        required: true
    },
    imgwidth: {
        type: Number,
        required: true
    },
    imgheight: {
        type: Number,
        required: true
    }
})

const northWest = {
    'lat': 49.910046,
    'lng': 7.226884
};

const southEast = {
    'lat': 47.275870,
    'lng': 10.778297
};

const x = ref(0)
const y = ref(0)

watch(() => props.imgwidth, (newwidth, oldwidth) => {
    const pixelsPerLng = newwidth / (southEast.lng - northWest.lng);
    x.value = ((props.location.lng - northWest.lng) * pixelsPerLng);
});

watch(() => props.imgheight, (newheight, oldheight) => {
    const pixelsPerLat = newheight / (northWest.lat - southEast.lat);
    y.value = (-1 * (props.location.lat - northWest.lat) * pixelsPerLat);
});
</script>

<template>
    <a href="#" v-if="x >= 0 && x <= imgwidth && y >= 0 && y <= imgheight" :style="{ top: y + 'px', left: x + 'px' }"></a>
</template>

<style scoped>
a {
    display: block;
    -webkit-border-radius: 100%;
    -moz-border-radius: 100%;
    border-radius: 100%;
    background: #ff1d58;
    position: absolute;
    width: 0.8em;
    height: 0.8em;
}
</style>