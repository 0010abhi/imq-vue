<!-- components/DirectionsMap.vue -->
<template>
    <div ref="map" style="height: 400px;"></div>
  </template>
  
  <script lang="ts">
  import { ref, onMounted } from '@vue/composition-api';
  
  export default {
    setup() {
      const map = ref(null);
  
      onMounted(() => {
        // Ensure that the google namespace is recognized by TypeScript
        if (typeof google === 'undefined') {
          throw new Error('Google Maps JavaScript API not loaded');
        }
  
        const directionsService = new google.maps.DirectionsService();
        const directionsRenderer = new google.maps.DirectionsRenderer();
  
        const mapOptions: google.maps.MapOptions = {
          center: { lat: 37.7749, lng: -122.4194 },
          zoom: 12,
        };
  
        const mapInstance = new google.maps.Map(map.value, mapOptions);
        directionsRenderer.setMap(mapInstance);
  
        fetchDirections(directionsService, directionsRenderer);
      });
  
      // Function to fetch directions
      const fetchDirections = (directionsService: google.maps.DirectionsService, directionsRenderer: google.maps.DirectionsRenderer) => {
        const request: google.maps.DirectionsRequest = {
          origin: 'San Francisco, CA',
          destination: 'Los Angeles, CA',
          travelMode: 'DRIVING',
        };
  
        directionsService.route(request, (result, status) => {
          if (status === 'OK') {
            directionsRenderer.setDirections(result);
          } else {
            console.error('Directions request failed due to ' + status);
          }
        });
      };
  
      return { map };
    },
  };
  </script>
  