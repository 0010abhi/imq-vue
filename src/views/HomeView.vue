<script setup lang="ts">
import { ref, type Ref } from 'vue';
import AppMap from '../components/AppMap.vue'
import AppLoader from "../components/AppLoader.vue"

import '../assets/main.css'

const query: Ref<string> = ref("");
const isLoading: Ref<boolean> = ref(false);
const response: any = ref(null);
const apiUrl = "http://localhost:3000/";
const body = {
  userString: query
};

const postData = async () => {
  isLoading.value = true;
  try {
    const options = {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        // You may need to include additional headers here (e.g., authorization headers)
      },
      body: JSON.stringify(body),
    };

    const fetchResponse = await fetch(apiUrl, options);

    if (!fetchResponse.ok) {
      throw new Error(`HTTP error! Status: ${fetchResponse.status}`);
    }

    const responseData = await fetchResponse.json();
    response.value = responseData;
    console.log(">>>", responseData)
  } catch (error) {
    console.error('Error making POST request:', error);
  }
  isLoading.value = false;
};



</script>

<template>
  <main style="width: 100%; border: 1px solid red">
    <div>
      <p>Interactive Map Query</p>
      <div>
        <textarea v-model="query" placeholder="Enter query for maps"></textarea>
      </div>
      <div>
        <button @click="postData">Click</button>
      </div>
      <AppLoader v-show="isLoading" />
      <div v-if="!isLoading && response?.opanAiResponse?.stringResponse">
        <div class="margin-b-15">
          {{ response.opanAiResponse.stringResponse }}
        </div>
        <div>
          <AppMap :mapData="response.routes" />
        </div>
      </div>
    </div>
  </main>
</template>