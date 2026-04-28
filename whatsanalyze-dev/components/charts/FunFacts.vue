<template>
  <div class="fun-facts">
    <div
      v-for="(person, idx) in funFacts"
      :key="idx"
      class="person-facts"
    >
      <div
        class="text-h4 font-weight-bold py-5"
        :style="{ color: 'white', background: person.color }"
      >
        {{ person.name }}
      </div>

      <div class="text-left mt-8">
        <div>
          <v-icon :color="person.color">mdi-book</v-icon>
          {{ $t("totalWords") }} <b>{{ person.numberOfWords }}</b>
        </div>

        <br />

        <!-- ✅ Fixed emoji rendering -->
        <div>
          <v-icon :color="person.color">
            mdi-emoticon-excited-outline
          </v-icon>
          {{ $t("mostUsedEmojie") }}

          <span
            v-for="(emoji, i) in person.sortedEmojis"
            :key="i"
            class="mx-1"
          >
            {{ emoji.emoji }} ({{ emoji.count }})
          </span>
        </div>

        <br />

        <div>
          <v-icon :color="person.color">mdi-android-messages</v-icon>
          {{ $t("longestMessage") }}
          <b>{{ person.longestMessage }}</b> words
        </div>

        <br />

        <div>
          <v-icon :color="person.color">mdi-star</v-icon>
          {{ $t("uniqueWords") }}
          <b>{{ person.uniqueWords }}</b>
        </div>

        <br />

        <div>
          <v-icon :color="person.color">mdi-android-studio</v-icon>
          {{ $t("avgWords") }}
          <b>{{ person.averageMessageLength }}</b>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Chat } from "~/utils/transformChatData";

export default {
  name: "ChatFunFacts",

  props: {
    chartdata: {
      type: Object,
      default: () => new Chat(),
    },
  },

  data() {
    return {
      funFacts: [],
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
      try {
        const data = await this.chartdata.getFunFacts();
        this.funFacts = data;
      } catch (error) {
        console.error("Error fetching fun facts:", error);
      }
    },
  },

  mounted() {
    this.updateGraph();
  },
};
</script>

<style lang="scss">
.fun-facts {
  overflow: hidden;
}

.person-facts {
  display: inline-block;
  margin: 1em;
  padding: 1em;
  border: 2px solid $c-white;
}
</style>