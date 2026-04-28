<script>
import { Chat } from "~/utils/transformChatData";

export default {
  name: "ChatDashboard",

  props: {
    chat: {
      type: Object,
      default: () => new Chat(),
    },

    attachments: {
      type: Array,
      default: () => [],
    },

    isValidSubscription: {
      type: Boolean,
      default: false,
    },
  },

  computed: {
    safeResults() {
      return {
        hasData: !!this.chat,
      };
    },
  },
};
</script>