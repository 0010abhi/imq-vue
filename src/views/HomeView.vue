<script setup lang="ts">
import AppMap from '../components/AppMap.vue'
</script>

<template>
  <main style="width: 100%;">
    <div>
      <p>Interactive Map Query</p>
      <div>
        <textarea placeholder="Enter query for maps"></textarea>
        <button>Click</button>
      </div>
      <div>
        <AppMap />
      </div>
    </div>
  </main>
</template>