<script setup>
import { ref, onMounted } from 'vue'

const queueTitle = 'Template:Did you know/Queue/5'
const url = 'https://en.wikipedia.org/w/api.php?origin=*&action=query&format=json&prop=revisions&formatversion=2&rvprop=content&rvslots=*&titles=' + queueTitle

const data = ref(null);
const error = ref(null);

function getQueue() {
  fetch(url)
    .then((res) => res.json())
    .then((json) => (data.value = json['query']['pages'][0]['revisions'][0]['slots']['main']['content']))
    .catch((err) => (error.value = err))
} 

onMounted(() => {
  getQueue();
})
</script>

<template>
    <div class="queue">
        {{ data }}
    </div>
  </template>
  
  <style>
  @media (min-width: 1024px) {
    .queue {
      min-height: 100vh;
      display: flex;
      align-items: center;
    }
  }
  </style>