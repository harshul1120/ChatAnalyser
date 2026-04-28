<script>
import { Chat } from "~/utils/transformChatData";
import { onlyEmoji } from "emoji-aware";

export default {
  name: "EmojiCloud",

  props: {
    chartdata: {
      type: Object,
      default: () => new Chat(),
    },

    minWordLength: {
      type: Number,
      default: 0,
    },

    minFontSize: {
      type: Number,
      default: 6,
    },

    randomness: {
      type: Number,
      default: 0.1,
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
    this.series.accuracy = 5;

    //  use props properly
    this.series.minFontSize = this.minFontSize;
    this.series.maxFontSize = 36;
    this.series.minWordLength = this.minWordLength;
    this.series.randomness = this.randomness;

    this.updateGraph();
  },

  beforeDestroy() {
    if (this.chart) {
      this.chart.dispose();
    }
  },

  methods: {
    async updateGraph() {
      const words = await this.chartdata.getEmojiCloudData();

      const filterPattern =
        /(?:€|\$|R\$|₹)?\d+[,.]?\d*(?:€|\$|R\$|₹)?|[!?]|^\.$/;

      const wordData = words.filter((wordObj) => {
        const isCurrency = filterPattern.test(wordObj.word);

        //  remove emojis instead of keeping them
        const isOnlyEmoji = onlyEmoji(wordObj.word).length > 0;

        return !isCurrency && !isOnlyEmoji;
      });

      this.series.data = wordData;
    },
  },
};
</script>