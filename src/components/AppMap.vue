<script setup lang="ts">
import { Loader } from '@googlemaps/js-api-loader';
import { ref, watch, type Ref } from 'vue';
const mapObject: Ref<any> = ref(null);
const directionObj: Ref<any> = ref(null);
const props = defineProps(['mapData']);
console.log('>>> props map', props, mapObject, directionObj)
const loader = new Loader({
    apiKey: "",
    // version: "weekly",
    libraries: ["places"]
});

const MapDiv: any = document.getElementById("map");

const mapOptions: any = {
    center: {
        lat: 0,
        lng: 0
    },
    zoom: 4
};
loader
    .load()
    .then((google) => {
        mapObject.value = new google.maps.Map(MapDiv, mapOptions);
        directionObj.value = new google.maps.DirectionsRenderer();
        directionObj.value.setMap(mapObject.value);
    })
    .catch(e => {
        console.error('Error in loading google map', e);

    });

watch(() => props.mapData, (newValue: any, oldValue: any) => {
    console.log(`Prop 'message' changed from ${oldValue} to ${newValue}`);
    directionObj.value.setDirections(newValue);
});
</script>

<template>
    <div id="map"></div>
</template>
  
<style scoped>
#map {
    height: 400px;
}
</style>
  