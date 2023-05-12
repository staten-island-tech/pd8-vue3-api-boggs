<template>
  <div>
    <ApexCharts type="bar" :options="barOptions" :series="barSeries" />
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

  if (!data) return {}

  const filteredData = data.filter((violation) => violation.violation !== undefined)

  const violationsByType = filteredData.reduce((accumulator, currentValue) => {
    const violationType = currentValue.violation
    if (!accumulator[violationType]) accumulator[violationType] = 0 // Initialize to 0 if it's a new violationType
    accumulator[violationType] += 1 // Add 1 for each instance of the violationType
    return accumulator
  }, {})

  const categories = Object.keys(violationsByType)
  const seriesData = Object.values(violationsByType)

  return {
    series: [
      {
        name: 'Number of Violations',
        data: seriesData
      }
    ],
    chart: {
      type: 'bar',
      height: 800,
      width: 1000
    },
    plotOptions: {
      bar: {
        horizontal: true,
        margin: 10,
        padding: 10
      }
    },
    theme: {
      mode: 'dark'
    },
    dataLabels: {
      enabled: false
    },
    xaxis: {
      categories: categories
    }
  }
})

const barSeries = computed(() => {
  const data = toRef(props, 'data').value
  if (!data) return []

  const filteredData = data.filter((violation) => violation.violation !== undefined)

  const violationsByType = filteredData.reduce((accumulator, currentValue) => {
    const violationType = currentValue.violation
    if (!accumulator[violationType]) accumulator[violationType] = 0 // Initialize to 0 if it's a new violationType
    accumulator[violationType] += 1 // Add 1 for each instance of the violationType
    return accumulator
  }, {})

  const seriesData = Object.values(violationsByType)

  return [
    {
      name: 'Number of Violations',
      data: seriesData
    }
  ]
})
</script>

<style scoped></style>
