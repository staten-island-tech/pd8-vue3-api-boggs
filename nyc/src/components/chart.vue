<template>
  <div>
    <ApexCharts type="bar" :options="barOptions" :series="barSeries" />
    <ApexCharts type="pie" :options="pieOptions" :series="pieSeries" />
  </div>
</template>

<script setup>
import { computed, toRef } from 'vue'
import ApexCharts from 'vue3-apexcharts'

const props = defineProps({
  data: Array
})

const barOptions = computed(() => {
  const data = toRef(props, 'data').value
  console.log(data)
  if (!data) return {}
  return {
    chart: {
      type: 'bar',
      height: 350
    },
    series: [
      {
        name: 'Number of Violations',
        data: data.map((violation) => violation.summons_number)
      }
    ],
    xaxis: {
      categories: data.map((violation) => violation.plate)
    }
  }
})

const barSeries = computed(() => {
  const data = toRef(props, 'data').value
  if (!data) return []
  return [
    {
      name: 'Number of Violations',
      data: data.map((violation) => violation.summons_number)
    }
  ]
})

const pieOptions = computed(() => {
  const data = toRef(props, 'data').value
  if (!data) return {}
  return {
    chart: {
      type: 'pie',
      height: 350
    },
    series: data.map((violation) => violation.fine_amount),
    labels: data.map((violation) => violation.violation)
  }
})

const pieSeries = computed(() => {
  const data = toRef(props, 'data').value
  if (!data) return []
  return data.map((violation) => violation.fine_amount)
})
</script>
