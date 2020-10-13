<template>
  <div id="app">
    <topBar v-if="$router.currentRoute.path !== '/loading'" />
    <router-view />
  </div>
</template>

<script>
import sassStyles from '@/components/chat/alt_chat_css.module.scss'
import topBar from "@/components/window/topBar";

export default {
  name: "App",
  components: {
    topBar
  },
  mounted() {}
};
</script>
