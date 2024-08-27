<script setup>
import { ref, onMounted } from 'vue'



const queueTitle = 'Template:Did you know/Queue/5'
const url = new URL('https://en.wikipedia.org/w/api.php');
url.search = new URLSearchParams({
    'origin': '*',
    'action': 'parse',
    'page': queueTitle,
    'prop': 'wikitext',
    'format': 'json',
    'formatversion': '2'
});

const data = ref(null);
const error = ref(null);

function getQueue() {
    fetch(url)
        .then((res) => res.json())
        .then((json) => (data.value = json['parse']['wikitext']))
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