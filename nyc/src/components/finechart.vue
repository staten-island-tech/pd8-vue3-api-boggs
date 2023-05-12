<template>
  <div>
    <ApexCharts type="pie" :options="pieOptions" :series="pieSeries" />
  </div>
</template>

<script setup>
import { computed, toRef } from 'vue'
import ApexCharts from 'vue3-apexcharts'

const props = defineProps({
  data: Array
})

const pieOptions = computed(() => {
  const violations = toRef(props, 'data').value
  if (!violations || violations.length === 0) return {}

  const filteredViolations = violations.filter((violation) => violation.violation !== undefined)

  const violationsByType = filteredViolations.reduce((accumulator, currentValue) => {
    const violationType = currentValue.violation
    if (!accumulator[violationType]) {
      accumulator[violationType] = 0
    }
    accumulator[violationType] += parseFloat(currentValue.payment_amount)
    return accumulator
  }, {})

  const totalPayment = Object.values(violationsByType).reduce((total, amount) => total + amount, 0)

  const seriesData = Object.values(violationsByType)
  const labels = Object.keys(violationsByType).map((type) => {
    const amount = violationsByType[type]
    const percentage = (amount / totalPayment) * 100
    return `${type} (${percentage.toFixed(2)}%)`
  })

  return {
    chart: {
      type: 'pie',
      height: 350
    },
    series: seriesData,
    labels,
    dataLabels: {
      enabled: filteredViolations.length < 20
    }
  }
})

const pieSeries = computed(() => {
  const violations = toRef(props, 'data').value
  if (!violations || violations.length === 0) return []

  const filteredViolations = violations.filter((violation) => violation.violation !== undefined)

  return filteredViolations.map((violation) => parseFloat(violation.payment_amount))
})
</script>
