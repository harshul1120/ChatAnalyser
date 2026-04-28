<script>
import { Radar } from "vue-chartjs";
import { Chat } from "~/utils/transformChatData";
import { updateAlpha } from "~/utils/colors";

export default {
  extends: Radar,

  props: {
    dataGrouping: {
      type: String,
      default: "daily",
      validator: (value) =>
        ["hourly", "daily", "weekly"].includes(value),
    },

    chartdata: {
      type: Object,
      default: () => new Chat(),
    },

    options: {
      type: Object,
      default: () => ({
        responsive: true,
        maintainAspectRatio: false,
        scale: {
          ticks: {
            beginAtZero: true,
            precision: 0,
          },
        },
        legend: {
          position: "bottom",
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

    options: {
      handler(newVal) {
        this.localOptions = JSON.parse(JSON.stringify(newVal));
      },
      deep: true,
    },
  },

  methods: {
    addOpacity(data) {
      data.datasets = data.datasets.map((dataset) => ({
        ...dataset,
        backgroundColor: updateAlpha(dataset.backgroundColor, 0.1),
      }));
      return data;
    },

    async updateGraph() {
      const map = {
        hourly: this.chartdata.getHourlyData,
        daily: this.chartdata.getDailyData,
        weekly: this.chartdata.getWeeklyData,
      };

      const fetchData = map[this.dataGrouping] || map.daily;

      const data = await fetchData.call(this.chartdata);

      const processedData = this.addOpacity(data);

      this.renderChart(processedData, this.localOptions);
    },
  },

  mounted() {
    this.updateGraph();
  },
};
</script>