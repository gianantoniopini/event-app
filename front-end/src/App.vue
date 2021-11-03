<template>
  <EventReminder />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import EventReminder from "./components/EventReminder.vue";

export default defineComponent({
  name: "App",
  components: {
    EventReminder,
  },
});
</script>
