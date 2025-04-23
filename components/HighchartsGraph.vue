<template>
  <div class="bg-white rounded-lg shadow-lg p-4">
    <h3 class="text-lg font-semibold mb-2">{{ title }}</h3>
    <div ref="chartContainer" class="w-full h-80"></div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, watch } from 'vue'
import Highcharts from 'highcharts'

const props = defineProps({
  title: {
    type: String,
    default: 'Análise de Dados'
  },
  chartType: {
    type: String,
    default: 'line'
  },
  chartData: {
    type: Array,
    default: () => []
  },
  categories: {
    type: Array,
    default: () => []
  }
})

const chartContainer = ref<HTMLElement | null>(null)
let chart: Highcharts.Chart | null = null

const initChart = () => {
  if (!chartContainer.value) return

  chart = Highcharts.chart(chartContainer.value, {
    chart: {
      type: props.chartType,
      backgroundColor: 'transparent'
    },
    title: {
      text: ''
    },
    xAxis: {
      categories: props.categories,
      title: {
        text: 'Período'
      }
    },
    yAxis: {
      title: {
        text: 'Valores'
      }
    },
    plotOptions: {
      series: {
        marker: {
          enabled: true
        }
      }
    },
    series: [{
      name: 'Dados',
      data: props.chartData,
      type: props.chartType as any
    }],
    credits: {
      enabled: false
    },
    tooltip: {
      shared: true,
      valueSuffix: ' unidades'
    }
  })
}

onMounted(() => {
  initChart()
})

onUnmounted(() => {
  if (chart) {
    chart.destroy()
  }
})

// Observar mudanças nos dados
watch(
  [() => props.chartData, () => props.categories, () => props.chartType],
  ([newData, newCategories, newType]) => {
    if (chart) {
      chart.update({
        chart: {
          type: newType as any
        },
        xAxis: {
          categories: newCategories
        },
        series: [{
          data: newData,
          type: newType as any
        }]
      })
    }
  },
  { deep: true }
)

// Exposição de métodos para uso externo
defineExpose({
  updateChart: (options: Highcharts.Options) => {
    if (!chart) return
    chart.update(options)
  }
})
</script>