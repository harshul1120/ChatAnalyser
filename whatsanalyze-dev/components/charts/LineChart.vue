<script>
import { Line } from "vue-chartjs";
import { Chat } from "~/utils/transformChatData";

export default {
  extends: Line,

  props: {
    chartdata: {
      type: Object,
      default: () => new Chat(),
    },

    options: {
      type: Object,
      default: () => ({
        pointHitRadius: 2,
        responsive: true,
        maintainAspectRatio: false,
        lineTension: 0,
        legend: {
          position: "bottom",
          display: false,
        },
        scales: {
          xAxes: [
            {
              type: "time",
              gridLines: {
                display: false,
              },
            },
          ],
          yAxes: [
            {
              scaleLabel: {
                display: true,
                labelString: "", // set later
              },
              ticks: {
                precision: 0,
                beginAtZero: true,
              },
            },
          ],
        },
        elements: {
          line: {
            tension: 0,
          },
        },
      }),
    },
  },

  data() {
    return {
      localOptions: JSON.parse(JSON.stringify(this.options)),
    };
  },

  watch: {
    chartdata: {
      handler() {
        this.updateGraph();
      },
      deep: true,
    },

    options: {
      handler(newVal) {
        this.localOptions = JSON.parse(JSON.stringify(newVal));
      },
      deep: true,
    },
  },

  methods: {
    async updateGraph() {
      const data = await this.chartdata.getLineGraphData();
      this.renderChart(data, this.localOptions);
    },
  },

  mounted() {
    // safe translation usage
    this.localOptions.scales.yAxes[0].scaleLabel.labelString =
      this.$t("messages");

    this.updateGraph();
  },
};
</script>
