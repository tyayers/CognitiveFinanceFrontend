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
        default: 'line'
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
          type: this.chartType,
          data: this.chartData,
          options: this.chartOptions
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
