<template>
  <div>
    <ApexCharts type="pie" :options="pieOptions" :series="pieSeries" />
    <!-- <ApexCharts type="line" :options="lineOptions" :series="lineSeries" /> -->
  </div>
</template>

<script setup>
import { computed, toRef } from 'vue'
import ApexCharts from 'vue3-apexcharts'

const props = defineProps({
  data: Array
})

const pieOptions = computed(() => {
  const data = toRef(props, 'data').value
  if (!data) return {}

  const violationsByType = data.reduce((accumulator, currentValue) => {
    const violationType = currentValue.violation
    if (!accumulator[violationType]) accumulator[violationType] = 0 // Initialize to 0 if it's a new violationType
    accumulator[violationType] += 1 // Add 1 for each instance of the violationType
    return accumulator
  }, {})
  const categories = Object.keys(violationsByType)
  const seriesData = Object.values(violationsByType)
  return {
    chart: {
      type: 'pie',
      height: 350
    },
    series: [
      {
        name: 'Number of Violations',
        data: seriesData
      }
    ],
    labels: data.map((violation) => violation.violation),
    dataLabels: {
      enabled: data.length < 20
    }
  }
})

const pieSeries = computed(() => {
  const data = toRef(props, 'data').value
  if (!data) return []
  return data.map((violation) => violation.payment_amount)
})

// const lineOptions = computed(() => {
//   const data = toRef(props, 'data').value
//   if (!data) return {}
//   return {
//     chart: {
//       type: 'line',
//       height: 350
//     },
//     series: [
//       {
//         name: 'Number of Violations',
//         data: data.map((violation) => violation.summons_number)
//       }
//     ],
//     xaxis: {
//       categories: data.map((violation) => violation.plate)
//     },
//     dataLabels: {
//       enabled: data.length < 20
//     }
//   }
// })

// const lineSeries = computed(() => {
//   const data = toRef(props, 'data').value
//   if (!data) return []
//   return [
//     {
//       name: 'Number of Violations',
//       data: data.map((violation) => violation.summons_number)
//     }
//   ]
// })
</script>
