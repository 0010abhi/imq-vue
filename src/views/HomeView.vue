<template>
  <main style="width: 100%; border: 1px solid red">
    <div>
      <div style="border: 1px solid green; display: flex; align-items: center; flex-direction: column;">
        <p class="block text-gray-700 text-sm font-bold mb-2">Interactive Map Query</p>
        <div class="m-8" style="width: 100%;">
          <textarea
            class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
            v-model="query" placeholder="Enter query for maps"></textarea>
        </div>
        <div>
          <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full"
            @click="postData">Click</button>
        </div>
      </div>
      <AppLoader v-show="isLoading" />
      <div style="border: 1px solid blue;" v-if="!isLoading && response">
        <div class="margin-b-15">
          {{ response?.opanAiResponse?.stringResponse }}
        </div>
        <div>
          <!-- {{ JSON.stringify(response?.mapResponse) }} -->
          <AppMap :mapData="response?.mapResponse" />
        </div>
      </div>
    </div>
  </main>
</template>


<script setup lang="ts">
import { ref, type Ref } from 'vue';
import AppMap from '../components/AppMap.vue'
import AppLoader from "../components/AppLoader.vue"

import '../assets/main.css'

const query: Ref<string> = ref("");
const isLoading: Ref<boolean> = ref(false);
const response: any = ref(null);
const apiUrl = "http://localhost:3000/";

const postData = async () => {
  isLoading.value = true;
  try {
    const options = {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        // You may need to include additional headers here (e.g., authorization headers)
      },
      body: JSON.stringify({
        userString: query.value
      }),
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

<style scoped></style>