<template>
  <div>
    <chart :data="cameraViolation.value" />
  </div>
</template>

<script setup>
import chart from '../components/chart.vue'
import { ref, onMounted } from 'vue'

const cameraViolation = ref('')

async function getViolation() {
  try {
    let res = await fetch('https://data.cityofnewyork.us/resource/nc67-uf89.json')
    let data = await res.json()
    cameraViolation.value = data
  } catch (error) {
    console.error(error)
  }
}

onMounted(() => {
  getViolation()
})
</script>
