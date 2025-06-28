<template>
  <div class="w-full h-full flex items-center justify-center">
    <canvas ref="chartCanvas" class="max-w-full max-h-full"></canvas>
  </div>
</template>

<script setup>
import { Chart, RadarController, RadialLinearScale, PointElement, LineElement, Filler, Tooltip, Legend } from 'chart.js'

Chart.register(RadarController, RadialLinearScale, PointElement, LineElement, Filler, Tooltip, Legend)

const props = defineProps({
  data: {
    type: Object,
    required: true
  }
})

const chartCanvas = ref(null)
let chartInstance = null

const createChart = () => {
  if (chartInstance) {
    chartInstance.destroy()
  }

  const ctx = chartCanvas.value.getContext('2d')
  
  chartInstance = new Chart(ctx, {
    type: 'radar',
    data: {
      labels: props.data.labels,
      datasets: [{
        label: 'Life Balance',
        data: props.data.scores,
        fill: true,
        backgroundColor: 'rgba(59, 130, 246, 0.1)',
        borderColor: 'rgba(59, 130, 246, 0.8)',
        pointBackgroundColor: 'rgba(59, 130, 246, 1)',
        pointBorderColor: '#fff',
        pointHoverBackgroundColor: '#fff',
        pointHoverBorderColor: 'rgba(139, 92, 246, 1)',
        borderWidth: 3,
        pointRadius: 6,
        pointHoverRadius: 8,
        tension: 0.1
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: true,
      animation: {
        duration: 1000,
        easing: 'easeOutQuart'
      },
      scales: {
        r: {
          angleLines: {
            display: true,
            color: 'rgba(0, 0, 0, 0.1)'
          },
          suggestedMin: 0,
          suggestedMax: 10,
          grid: {
            color: 'rgba(0, 0, 0, 0.1)'
          },
          pointLabels: {
            font: {
              size: 14,
              weight: '600'
            },
            color: '#374151',
            padding: 20
          },
          ticks: {
            display: true,
            stepSize: 2,
            color: '#9CA3AF',
            font: {
              size: 12
            }
          }
        }
      },
      plugins: {
        legend: {
          display: false
        },
        tooltip: {
          backgroundColor: 'rgba(255, 255, 255, 0.95)',
          titleColor: '#1F2937',
          bodyColor: '#374151',
          borderColor: 'rgba(59, 130, 246, 0.2)',
          borderWidth: 1,
          cornerRadius: 8,
          displayColors: false,
          callbacks: {
            title: function(context) {
              return context[0].label
            },
            label: function(context) {
              return `Score: ${context.parsed.r}/10`
            }
          }
        }
      },
      elements: {
        line: {
          borderWidth: 3
        },
        point: {
          radius: 6,
          hoverRadius: 8
        }
      }
    }
  })
}

watch(() => props.data, () => {
  if (chartInstance) {
    chartInstance.data.datasets[0].data = props.data.scores
    chartInstance.update('active')
  }
}, { deep: true })

onMounted(() => {
  nextTick(() => {
    createChart()
  })
})

onUnmounted(() => {
  if (chartInstance) {
    chartInstance.destroy()
  }
})
</script>