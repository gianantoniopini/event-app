<template>
  <Event />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Event from "./components/Event.vue";

export default defineComponent({
  name: "App",
  components: {
    Event,
  },
});
</script>
