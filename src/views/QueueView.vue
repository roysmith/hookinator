<template>
    <div class="queue">
      <h1>This is a queue</h1>
      <button @click="getUserInfo">UserInfo!</button>
      <p>
        {{ data }}
      </p>
    </div>
  </template>

<script setup>
import { ref, onMounted } from 'vue'

const url = 'https://en.wikipedia.org/w/api.php?origin=*&action=query&format=json&prop=revisions&titles=Template:Did you know/Queue/3&formatversion=2&rvprop=content&rvslots=*';

const data = ref(null);
const error = ref(null);

function getUserInfo() {
  fetch(url)
    .then((res) => res.text())
    .then((text) => (data.value = text))
    .catch((err) => (error.value = err))
}

onMounted(() => {
  getUserInfo();
})

</script>
  
  <style>
  @media (min-width: 1024px) {
    .queue {
      min-height: 100vh;
      display: flex;
      align-items: center;
    }
  }
  </style>