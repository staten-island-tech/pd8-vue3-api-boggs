<template>
  <div></div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
const accidents = ref('')
async function getAccidents() {
  let res = await fetch('https://data.cityofnewyork.us/resource/nc67-uf89.json')
  let data = await res.json()
  accidents.value = data.results
}
onMounted(() => {
  getAccidents()
})
</script>
<style></style>
