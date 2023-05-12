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
    chart: {
      type: 'bar',
      height: 600,
      width: 2000,
      plotOptions: {
        bar: {
          margin: 10,
          padding: 10
        }
      }
    },
    series: [
      {
        name: 'Number of Violations',
        data: seriesData
      }
    ],
    xaxis: {
      categories: categories,
      labels: {
        rotate: -45
      }
    },
    yaxis: {
      labels: {
        formatter: function (value) {
          return value.toFixed() // Remove 'M' from y-axis labels
        }
      }
    },
    dataLabels: {
      enabled: true,
      formatter: function (val) {
        return val.toFixed() // Remove 'M' from data labels
      },
      offsetY: -20,
      style: {
        fontSize: '12px',
        colors: ['#ffffff']
      }
    },
    tooltip: {
      enabled: true,
      custom: function ({ series, seriesIndex, dataPointIndex, w }) {
        const value = series[seriesIndex][dataPointIndex]
        const category = w.globals.labels[dataPointIndex]

        return `<div class="custom-tooltip">${category}: ${value}</div>`
      }
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

<style scoped>
apexcharts-tooltip {
  font-family: Arial, sans-serif;
  font-size: 12px;
  background-color: #000000 !important; /* Change to black and add !important */
  color: #ffffff;
  padding: 6px;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}
</style>
