<template>
  <Line
    :chart-options="chartOptions"
    :chart-data="chartData"
    :chart-id="chartId"
    :plugins="plugins"
    :css-classes="cssClasses"
    :styles="styles"
    :width="width"
    :height="height"
  />
</template>

<script lang="ts">
import { defineComponent, h, type PropType } from 'vue'

import { Line } from 'vue-chartjs'
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  LineElement,
  LinearScale,
  PointElement,
  CategoryScale,
  type Plugin
} from 'chart.js'

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  LineElement,
  LinearScale,
  PointElement,
  CategoryScale
)

export default defineComponent({
  name: 'LineChart',
  components: {
    Line
  },
  props: {
    chartId: {
      type: String,
      default: 'line-chart'
    },
    width: {
      type: Number,
      default: 400
    },
    height: {
      type: Number,
      default: 400
    },
    cssClasses: {
      default: '',
      type: String
    },
    styles: {
      type: Object as PropType<Partial<CSSStyleDeclaration>>,
      // eslint-disable-next-line @typescript-eslint/no-empty-function
      default: () => {}
    },
    plugins: {
      type: Array as PropType<Plugin<'line'>[]>,
      default: () => []
    }
  },
  setup(props) {
    const chartData = {
      labels: [
        'January;2015',
        'February;2015',
        'March;2015',
        'April;2015',
        'May;2015',
        'June;2015',
        'July;2015'
      ],
      datasets: [
        {
          label: 'Data One',
          backgroundColor: '#f87979',
          xAxisID: 'xAxis1',
          data: [40, 39, 10, 40, 39, 80, 40]
        }
      ]
    }

    const chartOptions = {
      responsive: true,
      maintainAspectRatio: false,
      scales: {
        x: {
          ticks: {
            callback: function (label: string) {
              const realLabel: string = this.getLabelForValue(label)
              console.log('🚀 ~ realLabel', realLabel)
              const month = realLabel.split(';')[0]
              const year = realLabel.split(';')[1]
              return month
            }
          } as {
            callback: () => string
            getLabelForValue: (label: string) => string
          }
        },
        xAxis2: {
          type: 'category',
          grid: {
            drawOnChartArea: false // only want the grid lines for one axis to show up
          },
          ticks: {
            callback: function (label: string) {
              const realLabel: string = this.getLabelForValue(label) as string

              const month = realLabel.split(';')[0]
              const year = realLabel.split(';')[1]
              if (month === 'February') {
                return year
              } else {
                return ''
              }
            }
          } as {
            callback: () => string
            getLabelForValue: (label: string) => string
          }
        },
        y: {
          beginAtZero: true
        }
      }
    }

    return () =>
      h(Line, {
        chartData,
        chartOptions,
        chartId: props.chartId,
        width: props.width,
        height: props.height,
        cssClasses: props.cssClasses,
        styles: props.styles,
        plugins: props.plugins
      })
  }
})
</script>
