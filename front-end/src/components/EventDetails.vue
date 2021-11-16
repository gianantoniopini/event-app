<template>
  <div class="row">
    <div class="col-sm-12 col-md-5">
      <div class="row">
        <div class="col-12 fw-light">Letzigrund Station, Zurich</div>
      </div>
      <div class="row">
        <div class="col-12">
          <div class="hstack gap-3">
            <span class="fs-3 text-nowrap">The Rolling Stones</span>
            <span class="rounded bg-primary text-white text-nowrap m-1 p-1">
              top event
            </span>
          </div>
        </div>
      </div>
      <div class="row mt-4">
        <div class="col-12">
          The stones roll through Europe this winter - The Rolling Stones will
          play at the Letzigrund stadium on
          {{ eventDateFormattedLong }}. On their "On Fire" tour, they will also
          make a stop in Zurich at the beginning of the winter... A heater and
          ticket prices are not yet known. We will inform you here and on social
          media. So check back from time to time.
        </div>
      </div>
      <div class="row mt-2 mt-md-5">
        <div class="col-12">
          <div class="container-fluid rounded text-white bg-dark py-4">
            <div class="row">
              <div class="col-12">
                <div class="hstack gap-2">
                  <i class="bi bi-clock"></i>
                  <div class="text-uppercase">Event Date</div>
                </div>
              </div>
            </div>
            <div class="row">
              <div class="col-12 fw-bold">
                {{ eventDateFormattedShort }} | {{ eventTimeFormatted }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="col-md-2"></div>
    <div class="col-sm-12 col-md-5 mt-2 mt-md-0 d-flex justify-content-md-end">
      <img
        src="../assets/disco-lights-small.jpg"
        width="640"
        height="512"
        srcset="
          ../assets/disco-lights-small.jpg   640w,
          ../assets/disco-lights-medium.jpg 1920w,
          ../assets/disco-lights-large.jpg  2400w
        "
        class="img-fluid rounded"
        alt="Assorted color disco lights"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { format as tzFormat, utcToZonedTime } from "date-fns-tz";

export default defineComponent({
  name: "EventDetails",

  props: {
    eventDateTime: {
      type: Date,
      required: true,
    },
    eventTimeZone: {
      type: String,
      required: true,
    },
  },

  computed: {
    zonedEventDateTime(): Date {
      return utcToZonedTime(this.eventDateTime.getTime(), this.eventTimeZone);
    },

    eventDateFormattedLong(): string {
      return tzFormat(this.zonedEventDateTime.getTime(), "EEEE, MMM dd yyyy");
    },

    eventDateFormattedShort(): string {
      return tzFormat(this.zonedEventDateTime.getTime(), "EEE dd. MMM");
    },

    eventTimeFormatted(): string {
      return tzFormat(this.zonedEventDateTime.getTime(), "HH:mm aa");
    },
  },
});
</script>
