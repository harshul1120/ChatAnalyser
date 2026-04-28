<script>
import { dateDiffs, firstDate, getDateString, lastDate } from "~/utils/utils";

export default {
  name: "ChatOverviewStats",

  props: {
    chat: {
      type: Object,
      default: () => null,
    },
  },

  computed: {
    firstDateValue() {
      return this.chat ? firstDate(this.chat) : null;
    },

    lastDateValue() {
      return this.chat ? lastDate(this.chat) : null;
    },

    firstDateString() {
      return this.firstDateValue
        ? getDateString(this.firstDateValue, false)
        : "-";
    },

    lastDateString() {
      return this.lastDateValue
        ? getDateString(this.lastDateValue, false)
        : "-";
    },

    totalChatDays() {
      return this.firstDateValue && this.lastDateValue
        ? dateDiffs(this.firstDateValue, this.lastDateValue)
        : 0;
    },

    totalMessages() {
      return this.chat?.chatObject?.length || 0;
    },
  },
};
</script>