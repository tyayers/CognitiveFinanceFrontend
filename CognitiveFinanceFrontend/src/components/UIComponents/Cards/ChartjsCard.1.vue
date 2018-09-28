<template>
  <div class="card">
    <div class="header">
      <slot name="title"></slot>
      <p class="category">
        <slot name="subTitle"></slot>
      </p>
    </div>
    <div class="content">
      <canvas :id="chartId" ></canvas>
      <div class="footer">
        <div class="chart-legend">
          <slot name="legend"></slot>
        </div>
        <hr>
        <div class="stats">
          <slot name="footer"></slot>
        </div>
        <div class="pull-right">
        </div>
      </div>
    </div>

  </div>
</template>
<script>
  export default {
    name: 'chartjs-card',
    props: {
      footerText: {
        type: String,
        default: ''
      },
      headerTitle: {
        type: String,
        default: 'Chart title'
      },
      chartType: {
        type: String,
        default: 'Line' // Line | Pie | Bar
      },
      chartOptions: {
        type: Object,
        default: () => {
          return {}
        }
      },
      chartData: {
        type: Object,
        default: () => {
          return {
            labels: [],
            series: []
          }
        }
      }
    },
    data () {
      return {
        chartId: 'no-id'
      }
    },
    methods: {
      /***
       * Initializes the chart by merging the chart options sent via props and the default chart options
       */
      initChart () {
        // var chartIdQuery = `#${this.chartId}`
        // this.$Chartist[this.chartType](chartIdQuery, this.chartData, this.chartOptions)
        var ctx = document.getElementById(this.chartId)
        var myChart = new this.$Chart(ctx, {
          type: 'bar',
          data: {
            labels: ['Red', 'Blue', 'Yellow', 'Green', 'Purple', 'Orange'],
            datasets: [{
              label: '# of Votes',
              data: [12, 19, 3, 5, 2, 3],
              backgroundColor: [
                'rgba(255, 99, 132, 0.2)',
                'rgba(54, 162, 235, 0.2)',
                'rgba(255, 206, 86, 0.2)',
                'rgba(75, 192, 192, 0.2)',
                'rgba(153, 102, 255, 0.2)',
                'rgba(255, 159, 64, 0.2)'
              ],
              borderColor: [
                'rgba(255,99,132,1)',
                'rgba(54, 162, 235, 1)',
                'rgba(255, 206, 86, 1)',
                'rgba(75, 192, 192, 1)',
                'rgba(153, 102, 255, 1)',
                'rgba(255, 159, 64, 1)'
              ],
              borderWidth: 1
            }]
          },
          options: {
            scales: {
              yAxes: [{
                ticks: {
                  beginAtZero: true
                }
              }]
            }
          }
        })
      },
      /***
       * Assigns a random id to the chart
       */
      updateChartId () {
        var currentTime = new Date().getTime().toString()
        var randomInt = this.getRandomInt(0, currentTime)
        this.chartId = `div_${randomInt}`
      },
      getRandomInt (min, max) {
        return Math.floor(Math.random() * (max - min + 1)) + min
      }
    },
    mounted () {
      this.updateChartId()
      this.$nextTick(this.initChart)
    }
  }

</script>
<style>

</style>
