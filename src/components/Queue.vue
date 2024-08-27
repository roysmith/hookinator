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

const hooks = ref(null);
const error = ref(null);

function getQueue() {
    fetch(url)
        .then((response) => response.json())
        .then((data) => {
            const wikitext = data['parse']['wikitext'];
            let inHooks = false;
            hooks.value = Array();
            for (const line of wikitext.split('\n')) {
                if (line == '<!--HooksEnd-->') {
                    inHooks = false;
                    continue;
                }
                if (inHooks) {
                    if (line[0] == '*') {
                        hooks.value.push(line);
                    }
                    continue;
                }
                if (line == '<!--Hooks-->') {
                    inHooks = true;
                    continue;
                }
            }
        })
        .catch((err) => (error.value = err));
}

onMounted(() => {
    getQueue();
})
</script>

<template>
    <div class="queue">
        <ol>
            <li v-for="hook in hooks">
                {{ hook }}
            </li>
        </ol>
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