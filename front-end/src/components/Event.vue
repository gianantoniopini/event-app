<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-sm-12 col-lg-8 offset-lg-2">
        <EventDetails v-bind:event="this.event" />
        <div class="row mt-2 my-md-5">
          <div class="col-12">
            <hr />
          </div>
        </div>
        <EventReminder v-bind:eventDateTime="this.event.dateTime" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { zonedTimeToUtc } from "date-fns-tz";
import EventInterface from "../interfaces/Event";
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
      event: {
        city: "Zurich",
        location: "Letzigrund Station, Zurich",
        venue: "Letzigrund stadium",
        continent: "Europe",
        band: this.getRandomBand(),
        tour: "On Fire",
        timeZone: "Europe/Berlin",
        season: "Winter",
        dateTime: new Date(),
      } as EventInterface,
    };
  },

  methods: {
    getRandomBand(): string {
      const bands = [
        "The Crazy Cows",
        "The Fat Horses",
        "The Mad Plunkers",
        "The Smart Bees",
      ];

      return bands[Math.floor(Math.random() * bands.length)];
    },

    setEventDateTime() {
      this.event.dateTime = zonedTimeToUtc(
        "2021-12-17 18:00:00.000",
        this.event.timeZone
      );
    },
  },

  mounted() {
    this.setEventDateTime();
  },
});
</script>
