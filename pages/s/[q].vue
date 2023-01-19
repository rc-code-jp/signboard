<template>
  <div class="page">
    {{ q }}
    <button class="fullscreen-button" @click="onFullscreen">
      full
    </button>
  </div>
</template>

<script lang="ts" setup>
import { onBeforeUnmount, onMounted } from 'vue';

const route = useRoute()

const q = decodeURIComponent(String(route.params.q))

const onFullscreen = () => {
  document.body.requestFullscreen();	
}

const wakeOn = async () => {
  let wakeLock = null;

  if ('wakeLock' in navigator) {
    console.log('supported browser.')
  } else {
    console.log('Wake lock is not supported by this browser.')
  }

  // create an async function to request a wake lock
  try {
    wakeLock = await window.navigator.wakeLock.request('screen');
  } catch (err) {
    console.error(err)
  }

  console.dir(wakeLock)
}

onMounted(() => {
  wakeOn()
})

onBeforeUnmount(async () => {
  let wakeLock = null;

  try {
    wakeLock = await window.navigator.wakeLock.request('release');
  } catch (err) {
    console.error(err)
  }

  console.dir(wakeLock)
})
</script>

<style scoped>
.page {
  -ms-writing-mode: tb-rl;
  writing-mode: vertical-rl;
  text-orientation: upright;
  height: 100vh;
  margin: 0 auto;
  text-align: center;
  font-size: 50vw;
  letter-spacing: 10vw;
}

.fullscreen-button {
  position: fixed;
  outline: none;
  border: none;
  background-color: transparent;
  font-size: 12px;
  top:1em;
  left: 1em;
}
</style>