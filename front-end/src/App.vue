<template>
  <div class="container-fluid py-2">
    <div class="row">
      <div class="d-flex justify-content-center align-items-center">
        <div class="hstack gap-1">
          <i class="bi bi-hand-thumbs-up-fill"></i>
          <div class="fw-bold">Event App</div>
        </div>
      </div>
    </div>
    <div class="row my-3">
      <hr class="border border-3" />
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
