<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-sm-12 col-lg-8 offset-lg-2">
        <EventDetails v-bind:eventDetails="this.eventDetails" />
        <div class="row mt-2 my-md-5">
          <div class="col-12">
            <hr />
          </div>
        </div>
        <EventReminder v-bind:eventDateTime="this.eventDetails.dateTime" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { format } from "date-fns";
import add from "date-fns/add";
import { zonedTimeToUtc } from "date-fns-tz";
import EventDetailsInterface from "../interfaces/EventDetails";
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
      eventDetails: {
        city: "Zurich",
        location: "Letzigrund Station, Zurich",
        venue: "Letzigrund stadium",
        continent: "Europe",
        band: this.getRandomBand(),
        tour: "On Fire",
        timeZone: "Europe/Berlin",
        season: "Winter",
        dateTime: new Date(),
      } as EventDetailsInterface,
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
      const eventDate = add(new Date(), { months: 3 });
      const eventDateFormatted = format(eventDate, "yyyy-MM-dd");

      this.eventDetails.dateTime = zonedTimeToUtc(
        `${eventDateFormatted} 18:00:00.000`,
        this.eventDetails.timeZone
      );
    },
  },

  mounted() {
    this.setEventDateTime();
  },
});
</script>
