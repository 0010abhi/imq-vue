<template>
    <div class="loader"></div>
</template>
  
<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
    name: 'AppLoader',
});
</script>
  
<style scoped>
.loader {
    border: 8px solid #3498db;
    border-top: 8px solid transparent;
    border-radius: 50%;
    width: 50px;
    height: 50px;
    animation: spin 1s linear infinite;
}

@keyframes spin {
    0% {
        transform: rotate(0deg);
    }

    100% {
        transform: rotate(360deg);
    }
}</style>
  