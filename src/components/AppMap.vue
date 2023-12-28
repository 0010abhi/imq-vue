<script setup lang="ts">
import { Loader } from '@googlemaps/js-api-loader';

const loader = new Loader({
    apiKey: "AIzaSyDeBJQecxX4ZjjPL6aWLogO5bwQusF4kZw",
    // version: "weekly",
    libraries: ["places"]
});

const mapOptions = {
    center: {
        lat: 0,
        lng: 0
    },
    zoom: 4
};
loader
    .load()
    .then((google) => {
        new google.maps.Map(document.getElementById("map"), mapOptions);
    })
    .catch(e => {
        console.error('Error in loading google map', e);

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
  