<template>
  <header
    class="drag select-none w-full h-10 z-0"
    style="z-index: 9999999999 !important"
  >
    <loadingIndicator />
    <statBar />
    <div :class="cssTopBar" style="z-index: 9999999999 !important">
      <div
        @click="openMenu"
        class="flex justify-start no-drag h-10 w-auto inline-block"
      >
        <div @contextmenu="handler($event)" :class="cssMenu">
          <div :class="cssTopBarBrand">
            <img
              class="w-4 h-4 mx-auto"
              :src="require('@/assets/zen-icon.png')"
            />
          </div>
          <transition name="fade">
            <div
              @mouseover="clearPending"
              @mouseleave="pendingClose"
              style="z-index: 9999999; top: 2.44rem"
              v-if="menu"
              class="absolute left-0 w-48 shadow-lg"
            >
              <div class="bg-gray-900 rounded-b">
                <router-link
                  :to="'/'"
                  class="block p-4 text-xs text-gray-200 hover:bg-gray-700 cursor-default"
                  >Messages</router-link
                >
                <router-link tag="div" :to="'/network'" :class="cssConnection">
                  <p :class="cssConnectionText">
                    {{
                      $store.getters.socketConnected
                        ? "Network"
                        : "Disconnected"
                    }}
                  </p>
                  <svg
                    :class="cssConnectionIcon"
                    viewBox="0 0 20 20"
                    fill="currentColor"
                  >
                    <path
                      fill-rule="evenodd"
                      d="M11.3 1.046A1 1 0 0112 2v5h4a1 1 0 01.82 1.573l-7 10A1 1 0 018 18v-5H4a1 1 0 01-.82-1.573l7-10a1 1 0 011.12-.38z"
                      clip-rule="evenodd"
                    />
                  </svg>
                </router-link>
                <router-link
                  :to="'/settings'"
                  class="block p-4 text-xs text-gray-200 hover:bg-gray-700 cursor-default"
                  >Preferences</router-link
                >
                <div
                  @click="$store.dispatch('forceQuit')"
                  class="block p-4 text-xs text-gray-200 hover:bg-gray-700 rounded-b cursor-default"
                >
                  Force Quit
                </div>
              </div>
            </div>
          </transition>
        </div>
      </div>
    </div>
  </header>
</template>
<script>
import loadingIndicator from "@/components/window/loadingIndicator";
const ipc = require("electron").ipcRenderer;
import statBar from "@/components/window/statBar";

export default {
  name: "topBar",
  components: {
    loadingIndicator,
    statBar
  },
  data() {
    return {
      menu: false,
      wait: null
    };
  },
  methods: {
    handler: function(e) {
      if (this.$store.getters.socketAuth) {
        this.$router.push("/");
      }
      e.preventDefault();
    },
    clearPending() {
      if (this.wait !== null) {
        clearTimeout(this.wait);
      }
    },
    openMenu() {
      if (!this.$store.getters.socketAuth) return;
      this.menu = !this.menu;

      if (this.menu == true) {
        this.wait = null;
      }
    },
    pendingClose() {
      this.wait = setTimeout(this.closeMenu, 1000);
    },
    closeMenu() {
      this.wait = null;
      this.menu = false;
    }
  },
  computed: {
    cssMenu() {
      if (
        this.$store.getters.socketConnected &&
        this.$store.getters.socketAuth
      ) {
        return "group relative flex border-b-2 border-green-500";
      } else {
        return "group relative flex border-b-2 border-red-500";
      }
    },
    cssConnection() {
      if (
        this.$store.getters.socketConnected &&
        this.$store.getters.socketAuth
      ) {
        return "hover:bg-gray-700 block flex justify-between p-4 text-xs bg-gray-900 cursor-default";
      } else {
        return "block flex justify-between p-4 text-xs bg-gray-900 cursor-default";
      }
    },
    cssConnectionText() {
      if (
        this.$store.getters.socketConnected &&
        this.$store.getters.socketAuth
      ) {
        return "my-auto text-gray-300";
      } else {
        return "my-auto text-red-400";
      }
    },
    cssConnectionIcon() {
      if (
        this.$store.getters.socketConnected &&
        this.$store.getters.socketAuth
      ) {
        return "my-auto w-5 h-5 text-green-300 p-1 rounded bg-opacque-white fill-current";
      } else {
        return "my-auto w-5 h-5 text-red-400 p-1 rounded bg-opacque-white fill-current";
      }
    },
    cssTopBar() {
      if (this.$store.getters.isDark) {
        return "mx-auto bg-gray-900 w-full h-10 flex justify-between border-b border-gray-600";
      } else {
        return "mx-auto bg-gray-900 w-full h-10 flex justify-between border-b border-gray-800";
      }
    },
    cssTopBarBrand() {
      let c = "w-48 h-10 my-auto flex p-3";
      if (this.$store.getters.isDark) {
        return `${c} bg-gray-800 hover:bg-gray-700`;
      } else {
        if (this.menu) {
          return `${c} bg-gray-800`;
        } else {
          if (!this.$store.getters.socketAuth) {
            return `${c} `;
          } else {
            return `${c} hover:bg-gray-700`;
          }
        }
      }
    }
  }
};
</script>
<style scoped>
.router-link-exact-active {
  background: #1c64f2 !important;
}

.bg-opacque-white {
  background: rgba(255, 255, 255, 0.1);
}

.bg-opacque-black {
  background: rgba(0, 0, 0, 0.1);
}

.fade-enter-active,
.fade-leave-active {
  transform: translateY(0.01px);
  transition: all 150ms ease-in-out;
}

.fade-enter,
.fade-leave-to

/* .fade-leave-active below version 2.1.8 */
 {
  opacity: 0;
  transform: translateY(-5px);
}
</style>
