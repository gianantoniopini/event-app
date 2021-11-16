<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-sm-12 col-lg-8 offset-lg-2">
        <EventDetails
          v-bind:eventDateTime="this.eventDateTime"
          v-bind:eventTimeZone="this.eventTimeZone"
        />
        <div class="row mt-2 my-md-5">
          <div class="col-12">
            <hr />
          </div>
        </div>
        <EventReminder v-bind:eventDateTime="this.eventDateTime" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { zonedTimeToUtc } from "date-fns-tz";
import EventDetails from "./EventDetails.vue";
import EventReminder from "./EventReminder.vue";

export default defineComponent({
  name: "Event",

  components: {
    EventDetails,
    EventReminder,
  },

  data() {
    return {
      eventDateTime: new Date(),
      eventTimeZone: "",
    };
  },

  methods: {
    setEventDateTime() {
      this.eventTimeZone = "Europe/Berlin";
      this.eventDateTime = zonedTimeToUtc(
        "2021-12-17 18:00:00.000",
        this.eventTimeZone
      );
    },
  },

  mounted() {
    this.setEventDateTime();
  },
});
</script>
