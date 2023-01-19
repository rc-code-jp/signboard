<template>
  <div class="page">
    <input type="text" v-model="q" maxlength="2">
    <div>
      <button @click="goView">
        go
      </button>
    </div>
  </div>
</template>

<script lang="ts" setup>
const router = useRouter()

const q = ref('')

const goView = () => {
  return router.push({
    name: 's-q',
    params: {
      q: encodeURIComponent(q.value)
    }
  })
}

</script>

<style scoped>
.page {
  display: flex;
  flex: row nowrap;
  justify-content: center;
  align-items: center;
  height: 100svh;
}

input {
  outline: none;
  border: 1px solid black;
  padding: 0.5em 1em;
  font-size: 18px;
}

button {
  outline: none;
  border: 1px solid black;
  padding: 0.5em 1em;
  font-size: 18px;
}
</style>