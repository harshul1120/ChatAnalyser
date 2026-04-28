<script>
import { Chat } from "~/utils/transformChatData";
import { withoutEmoji } from "emoji-aware";
import stopwords from "stopwords-de";

export default {
  name: "WordCloud",

  props: {
    chartdata: {
      type: Object,
      default: () => new Chat(),
    },

    minWordLength: {
      type: Number,
      default: 3,
    },

    minFontSize: {
      type: Number,
      default: 6,
    },

    randomness: {
      type: Number,
      default: 0.1,
    },

    stopWords: {
      type: Array,
      default: () => stopwords,
    },
  },

  data() {
    return {
      chart: null,
      series: null,
    };
  },

  watch: {
    chartdata: {
      handler() {
        this.updateGraph();
      },
      deep: true,
    },
  },

  methods: {
    async updateGraph() {
      const words = await this.chartdata.getAllWords();

      const wordData = words.filter((wordObj) => {
        const cleanWord = withoutEmoji(wordObj.word).toLowerCase();

        const isValidLength = cleanWord.length >= this.minWordLength;
        const isNotStopWord = !this.stopWords.includes(cleanWord);

        return cleanWord && isValidLength && isNotStopWord;
      });

      this.series.data = wordData;
    },
  },

  mounted() {
    const { am4core, am4themes_animated, am4plugins_wordCloud } =
      this.$am4core();

    am4core.useTheme(am4themes_animated);
    am4core.options.onlyShowOnViewport = true;

    this.chart = am4core.create(
      this.$refs.chartdiv,
      am4plugins_wordCloud.WordCloud
    );

    this.series = this.chart.series.push(
      new am4plugins_wordCloud.WordCloudSeries()
    );

    this.series.dataFields.word = "word";
    this.series.dataFields.value = "freq";

    this.series.labels.template.tooltipText = "[bold]{freq}[/] x {word}";
    this.series.accuracy = 4;

    // ✅ use props properly
    this.series.minFontSize = this.minFontSize;
    this.series.minWordLength = this.minWordLength;
    this.series.randomness = this.randomness;

    this.updateGraph();
  },

  beforeDestroy() {
    if (this.chart) {
      this.chart.dispose();
    }
  },
};
</script>