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
        'February;2016',
        'March;2017',
        'April;2018',
        'May;2019',
        'June;2020',
        'July;2021'
      ],
      datasets: [
        {
          label: 'Data One',
          backgroundColor: '#f87979',
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
              const month = realLabel.split(';')[0]
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
              const year = realLabel.split(';')[1]
              return year
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
