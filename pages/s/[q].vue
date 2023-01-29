<template>
  <div class="page">
    <div ref="$container" class="container">
      {{ q }}
    </div>
    <NuxtLink class="back-button" :to="{name: 'index'}">
      <img src="@/assets/images/back.svg" alt="Rotate">
    </NuxtLink>
    <a href="#" class="rotate-button" @click.prevent="rotate">
      <img src="@/assets/images/rotate.svg" alt="Rotate">
    </a>
  </div>
</template>

<script lang="ts" setup>
import { onBeforeUnmount, onMounted, ref } from 'vue';

const route = useRoute()

const q = decodeURIComponent(String(route.params.q))

const $container = ref<HTMLDivElement>()

const rotate = () => {
  if ($container.value?.style.getPropertyValue('rotate') === '180deg') {
    $container.value?.style.removeProperty('rotate')
  } else {
    $container.value?.style.setProperty('rotate', '180deg')
  }
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

  $container.value?.style.removeProperty('rotate')
})
</script>

<style scoped>
.container {
  -ms-writing-mode: tb-rl;
  writing-mode: vertical-rl;
  text-orientation: upright;
  height: 100svh;
  margin: 0 auto;
  text-align: center;
  font-size: 50vw;
  letter-spacing: 10vw;
  transition: rotate 0.3s ease;
}

.back-button {
  display: flex;
  position: fixed;
  outline: none;
  border: none;
  background-color: transparent;
  text-decoration: none;
  color: darkgray;
  width: 1em;
  height: 1em;
  top: 0.75em;
  left: 1em;
}

.rotate-button {
  display: flex;
  position: fixed;
  outline: none;
  border: none;
  background-color: transparent;
  text-decoration: none;
  color: darkgray;
  width: 1em;
  height: 1em;
  top: 0.75em;
  right: 1em;
}

@media screen and (min-width: 600px) {
  .container {
    font-size: 30vh;
    letter-spacing: 10vh;
  }
}
</style>