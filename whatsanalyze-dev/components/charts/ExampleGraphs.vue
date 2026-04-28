<script>
import { Chat } from "~/utils/transformChatData";

export default {
  name: "ExampleGraphs",

  data() {
    return {
      chat: null,

      linegraphHeaderChartOptions: {
        tooltips: { enabled: false },
        hover: { mode: null },
        responsive: true,
        maintainAspectRatio: true,
        legend: {
          position: "top",
        },
        scales: {
          xAxes: [
            {
              type: "time",
              gridLines: { display: false },
            },
          ],
          yAxes: [
            {
              scaleLabel: {
                display: true,
                labelString: "", // set later
              },
              ticks: {
                beginAtZero: true,
              },
            },
          ],
        },
      },

      donoughtHeaderChartOptions: {
        responsive: true,
        maintainAspectRatio: true,
        legend: {
          position: "bottom",
        },
      },

      radarchartHeaderChartOptions: {
        responsive: true,
        maintainAspectRatio: true,
        legend: {
          position: "top",
        },
      },

      barchartHeaderChartOptions: {
        responsive: true,
        maintainAspectRatio: true,
        legend: {
          position: "bottom",
        },
      },
    };
  },

  mounted() {
    // ✅ safe place for translations
    const label = this.$t("messages");

    this.linegraphHeaderChartOptions.scales.yAxes[0].scaleLabel.labelString =
      label;
  },

  created() {
    if (process.client) {
      fetch("/example-results.json")
        .then((res) => res.text())
        .then((messages) => {
          const instance = new Chat();
          const data = JSON.parse(messages);

          Object.assign(instance, {
            _lineGraphData: Promise.resolve(data[0]),
            _funfacts: Promise.resolve(data[1]),
            _allWords: Promise.resolve(data[2]),
            _hourlyData: Promise.resolve(data[3]),
            _dailyData: Promise.resolve(data[4]),
            _weeklyData: Promise.resolve(data[5]),
            _shareOfSpeech: Promise.resolve(data[6]),
          });

          this.chat = instance;
        });
    }
  },
};
</script>