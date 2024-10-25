<script setup lang="ts">
import { RouterLink, RouterView } from "vue-router";
import WebDispenserHeader from "./components/WebDispenserHeader.vue";
import WebDispenserFooter from "./components/WebDispenserFooter.vue";
</script>

<template>
  <WebDispenserHeader />
  <RouterView />
  <WebDispenserFooter />
</template>
