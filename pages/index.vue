<template>
  <div>
    <input type="text" v-model="q">
    <div>
      <button @click="setText">
        go
      </button>
    </div>
  </div>
</template>

<script lang="ts" setup>
const router = useRouter()

const q = ref('')

const setText = () => {
  router.push({
    name: 's',
    params: {
      q: encodeURIComponent(q.value)
    }
  })
}

</script>