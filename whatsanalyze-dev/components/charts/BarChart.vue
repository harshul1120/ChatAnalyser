<script>
import { Bar } from "vue-chartjs";
import { Chat } from "~/utils/transformChatData";

export default {
  extends: Bar,

  props: {
    chartdata: {
      type: Object,
      default: () => new Chat(),
    },

    dataGrouping: {
      type: String,
      default: "daily",
      validator: (value) => ["hourly", "daily", "weekly"].includes(value),
    },

    options: {
      type: Object,
      default: () => ({
        responsive: true,
        maintainAspectRatio: false,
        legend: {
          position: "bottom",
        },
        scales: {
          xAxes: [
            {
              gridLines: {
                display: false,
              },
            },
          ],
          yAxes: [
            {
              scaleLabel: {
                display: true,
                labelString: "", // will set later (no this here)
              },
              ticks: {
                beginAtZero: true,
                precision: 0,
              },
            },
          ],
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
    dataGrouping() {
      this.updateGraph();
    },
  },

  methods: {
    setStacked(startStackingAt = 4) {
      const stacked = this.chartdata.numPersonsInChat > startStackingAt;

      this.localOptions.scales.xAxes[0].stacked = stacked;
      this.localOptions.scales.yAxes[0].stacked = stacked;
    },

    async updateGraph() {
      this.setStacked();

      const map = {
        hourly: this.chartdata.getHourlyData,
        daily: this.chartdata.getDailyData,
        weekly: this.chartdata.getWeeklyData,
      };

      const fetchData = map[this.dataGrouping] || map.daily;

      const data = await fetchData.call(this.chartdata);

      this.renderChart(data, this.localOptions);
    },
  },

  mounted() {
    // safe place to use this.$t
    this.localOptions.scales.yAxes[0].scaleLabel.labelString =
      this.$t("messages");

    this.updateGraph();
  },
};
</script>