<script>
import VueCharts from 'vue-chartjs'
import { Pie, Bar, mixins } from 'vue-chartjs'
import axios from 'axios'

export default {
  mixins: [mixins.reactiveData],
  data() {
    return {
      chartData: '',
      options: {
          scales: {
            yAxes: [{
              ticks: {
                beginAtZero: true
              },
              gridLines: {
                display: true
              }
            }],
            xAxes: [ {
              gridLines: {
                display: false
              }
            }]
          },
          legend: {
            display: true
          },
          responsive: true,
          maintainAspectRatio: false
        }
    }
  },
  extends: Bar,
  mounted() {
    this.renderChart(this.chartData, this.options)
  },
  created() {
    axios.get(`http://localhost:8010/proxy/wf.json`)
      .then(response => {
        // JSON responses are automatically parsed.
        const responseData = response.data
        this.chartData = {
          labels: responseData.map(item => item.YEAR),
          datasets: [{
            label: 'Daily Students',
             backgroundColor: '#f87979',
             data: responseData.map(item => item.FREQ)

          }]
        }
      })
  }
}
</script>