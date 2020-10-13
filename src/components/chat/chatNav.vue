<template>
  <div>


    <section
      class="select-none overflow-y-scroll  left-0 bg-gray-200 px-2 py-2 flex flex-col"
      style="height: calc(100vh - 2.5em); top: 2.5rem"
    >
      <chatNavItem v-for="(t, ta) in contacts" :key="ta" :chat="t" />
    </section>
    <statBar />
  </div>
</template>

<script>
import chatNavItem from "@/components/chat/chatNavItem";
import statBar from "@/components/window/statBar";

export default {
  name: "chatNav",
  components: {
    chatNavItem,
    statBar
  },
  methods: {
    handler: function(e) {
      e.preventDefault();
    },
    onTarget: function(e) {
      this.$emit("setTarget", e);
    }
  },
  watch: {
    attachmentDrawer: {
      handler: function(x, y) {
        this.$store.commit("setLoading", true);
        setTimeout(() => {
          this.$store.commit("setLoading", false);
        }, 500);
      }
    }
  },
  computed: {
    contacts() {
      return this.$store.getters.contactList
        ? this.$store.getters.contactList
        : [];
    },
    handleName() {
      return this.$store.getters.getTargetIdentifier;
    },
    cssAttachmentDrawerButton() {
      let c;
      if (this.attachmentDrawer) {
        c = `bg-blue-300 text-blue-700`;
      } else {
        c = `bg-gray-300 text-gray-600`;
      }
      return `${c} w-8 h-8 my-auto rounded p-2 hover:opacity-75 cursor-pointer fill-current`;
    }
  },

  data() {
    return {
      attachmentDrawer: false
    };
  }
};
</script>

<style>
.person-tile {
  transition: all 140ms ease;
}

::-webkit-scrollbar {
  width: 1px;
  /* Remove scrollbar space */
  background: rgba(0, 0, 0, 0.05);
  border-radius: 2px;
  /* Optional: just make scrollbar invisible */
}

::-webkit-scrollbar-thumb {
  width: 1px;
  background: rgba(24, 24, 24, 0.1);
}

.attachmentDrawer-enter-active,
.attachmentDrawer-leave-active {
  transform: translateY(-0.001px);
  opacity: 1;
  transition: all 190ms ease;
}

.attachmentDrawer-enter,
.attachmentDrawer-leave-to

/* .fade-leave-active below version 2.1.8 */
 {
  transform: translateY(-4em);
  opacity: 0;
}
</style>
