<script setup lang="ts">
import { Loader } from '@googlemaps/js-api-loader';
import { ref, watch, type Ref } from 'vue';
const mapObject: Ref<any> = ref(null);
const directionObj: Ref<any> = ref(null);
const props = defineProps(['mapData']);
console.log('>>> props map', props.mapData[0].overview_polyline)
const MapDiv: any = document.getElementById("map");

async function initMap() {
    // Request needed libraries.
    const { Map, Polyline } = await google.maps.importLibrary("maps") as google.maps.MapsLibrary;
    const { AdvancedMarkerElement } = await google.maps.importLibrary("marker") as google.maps.MarkerLibrary;
    const {DirectionsService} = await google.maps.importLibrary("routes") as google.maps.RoutesLibrary;
    
    const map = new Map(document.getElementById('map') as HTMLElement, {
        center: { lat: 37.4239163, lng: -122.0947209 },
        zoom: 14,
        mapId: '4504f8b37365c3d0',
    });

    // const marker = 
    new AdvancedMarkerElement({
        map,
        position: props.mapData.bounds.northeast,
    });


    // const marker = 
    new AdvancedMarkerElement({
        map,
        position: props.mapData.bounds.southwest,
    });

    const directionsRenderer = new google.maps.DirectionsRenderer();
        directionsRenderer.setMap(map);

        const directionsService = new google.maps.DirectionsService();
        directionsService.route(
          {
            origin,
            destination,
            travelMode: 'DRIVING',
          },
          (result, status) => {
            if (status === 'OK') {
              directionsRenderer.setDirections(result);
            } else {
              console.error('Directions request failed due to ' + status);
            }
          }
        );

//     const flightPath = new Polyline({
//     path: props.mapData[0].overview_polyline,
//     geodesic: true,
//     strokeColor: "#FF0000",
//     strokeOpacity: 1.0,
//     strokeWeight: 2,
//   });

//   flightPath.setMap(map);


}

initMap();

// const mapOptions: any = {
//     center: {
//         lat: 0,
//         lng: 0
//     },
//     zoom: 4
// };
// loader
//     .load()
//     .then((google) => {
//         mapObject.value = new google.maps.Map(MapDiv, mapOptions);
//         const marker = new google.maps.marker.AdvancedMarkerElement({
//             // mapObject.value,
//             position: { lat: 37.4239163, lng: -122.0947209 },
//         });
//         directionObj.value = new google.maps.DirectionsRenderer();
//         directionObj.value.setMap(mapObject.value);
//     })
//     .catch(e => {
//         console.error('Error in loading google map', e);

//     });
// console.log('>>> hello', props.mapData)
// watch(() => props.mapData, (newValue: any, oldValue: any) => {
//     console.log(`Prop 'map data' changed from ${oldValue} to ${newValue}`);
    
// });
</script>

<template>
    <div id="map"></div>
</template>
  
<style scoped>
#map {
    height: 400px;
}
</style>
  