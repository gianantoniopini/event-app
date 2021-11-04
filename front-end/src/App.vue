<template>
  <div class="container-fluid">
    <div class="row mt-2">
      <div class="d-flex justify-content-center align-items-center fw-bold">
        Event App
      </div>
    </div>
    <div class="row mt-3">
      <hr />
    </div>
    <EventReminder />
  </div>
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
