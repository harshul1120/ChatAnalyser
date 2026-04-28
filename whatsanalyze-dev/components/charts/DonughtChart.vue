<script>
import { Doughnut } from "vue-chartjs";
import { Chat } from "~/utils/transformChatData";

export default {
  extends: Doughnut,

  props: {
    chartdata: {
      type: Object,
      default: () => new Chat(),
    },

    options: {
      type: Object,
      default: () => ({
        responsive: true,
        maintainAspectRatio: false,
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

    options: {
      handler(newVal) {
        this.localOptions = JSON.parse(JSON.stringify(newVal));
      },
      deep: true,
    },
  },

  methods: {
    async updateGraph() {
      const data = await this.chartdata.getShareOfSpeech();
      this.renderChart(data, this.localOptions);
    },
  },

  mounted() {
    this.updateGraph();
  },
};
</script>