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
    <div class="row mt-4">
      <div class="col-8 offset-2">
        <div class="row">
          <div class="col-12">
            <div class="row">
              <div class="col-12">
                <div>Event Date</div>
                <div>{{ eventDateFormattedLong }}</div>
                <div>
                  {{ eventDateFormattedShort }} | {{ eventTimeFormatted }}
                </div>
              </div>
            </div>
            <div class="row mt-5">
              <div class="col-12">
                <hr />
              </div>
            </div>
            <div class="row mt-5">
              <form @submit.prevent="onSubmit" class="col-12 form">
                <div class="row">
                  <div class="col-12 fw-bold">Set a check back reminder</div>
                </div>
                <div class="row mt-3">
                  <div class="col-5">
                    <input
                      class="form-control"
                      type="date"
                      v-model="reminderDate"
                      :class="{
                        'is-invalid': this.reminderDateTimeValidationError,
                      }"
                      data-toggle="tooltip"
                      :title="this.reminderDateTimeValidationError"
                      required
                    />
                  </div>
                  <div class="col-2">
                    <input
                      class="form-control text-center"
                      type="time"
                      v-model="reminderTime"
                      :class="{
                        'is-invalid': this.reminderDateTimeValidationError,
                      }"
                      data-toggle="tooltip"
                      :title="this.reminderDateTimeValidationError"
                      required
                    />
                  </div>
                  <div class="col-5">
                    <input
                      class="form-control text-center"
                      type="number"
                      v-model="reminderDays"
                      required
                    />
                  </div>
                </div>
                <div class="row mt-2">
                  <div class="col-12">
                    <input
                      class="form-control"
                      type="email"
                      v-model="reminderEmail"
                      placeholder="Enter your email"
                    />
                  </div>
                </div>
                <div class="row mt-2">
                  <div class="col-12 btn-group d-flex" role="group">
                    <button
                      :disabled="invalid"
                      type="submit"
                      class="btn btn-primary"
                      @click="handleSubmit"
                    >
                      Submit
                    </button>
                  </div>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { format } from "date-fns";
import add from "date-fns/add";
import parseISO from "date-fns/parseISO";
import differenceInCalendarDays from "date-fns/differenceInCalendarDays";
import {
  zonedTimeToUtc,
  format as tzFormat,
  utcToZonedTime,
} from "date-fns-tz";

export default defineComponent({
  name: "EventReminder",

  data() {
    return {
      eventDateTime: new Date(),
      eventDateFormattedLong: "",
      eventDateFormattedShort: "",
      eventTimeFormatted: "",
      reminderDate: "",
      reminderTime: "13:00",
      reminderDateTimeValidationError: "",
      reminderDays: 5,
      reminderEmail: "",
    };
  },

  watch: {
    reminderDate() {
      this.validateReminderDateTime();

      const reminderDays = this.calculateReminderDays();

      if (this.reminderDays === reminderDays) {
        return;
      }

      this.reminderDays = reminderDays;
    },

    reminderTime() {
      this.validateReminderDateTime();
    },

    reminderDays() {
      const reminderDate = this.calculateReminderDate();

      if (this.reminderDate === reminderDate) {
        return;
      }

      this.reminderDate = reminderDate;
    },
  },

  computed: {
    invalid(): boolean {
      return this.reminderDateTimeValidationError.length > 0;
    },
  },

  methods: {
    calculateReminderDate() {
      const reminderDate = add(new Date(), { days: this.reminderDays });

      return format(reminderDate, "yyyy-MM-dd");
    },

    calculateReminderDays() {
      const reminderDate = parseISO(this.reminderDate);

      const reminderDays = differenceInCalendarDays(reminderDate, new Date());

      return reminderDays;
    },

    handleSubmit() {
      this.validateReminderDateTime();

      if (this.invalid) {
        return;
      }

      console.log("submitting form values...");
    },

    setEventDateTime() {
      const eventTimeZone = "Europe/Berlin";
      this.eventDateTime = zonedTimeToUtc(
        "2021-12-17 18:00:00.000",
        eventTimeZone
      );

      const zonedEventDateTime = utcToZonedTime(
        this.eventDateTime.getTime(),
        eventTimeZone
      );

      this.eventDateFormattedLong = tzFormat(
        zonedEventDateTime.getTime(),
        "EEEE, MMM dd yyyy"
      );
      this.eventDateFormattedShort = tzFormat(
        zonedEventDateTime.getTime(),
        "EEE dd. MMM"
      );
      this.eventTimeFormatted = tzFormat(
        zonedEventDateTime.getTime(),
        "HH:mm aa"
      );
    },

    validateReminderDateTime() {
      const reminderDateTime = parseISO(this.reminderDate);
      const reminderHours = parseInt(this.reminderTime.split(":")[0]);
      const reminderMinutes = parseInt(this.reminderTime.split(":")[1]);
      reminderDateTime.setHours(reminderHours);
      reminderDateTime.setMinutes(reminderMinutes);

      this.reminderDateTimeValidationError = "";

      if (reminderDateTime > this.eventDateTime) {
        this.reminderDateTimeValidationError =
          "The reminder date cannot be past the event date";
      } else if (reminderDateTime <= new Date()) {
        this.reminderDateTimeValidationError =
          "The reminder date cannot be in the past";
      }
    },
  },

  mounted() {
    this.setEventDateTime();
    this.reminderDate = this.calculateReminderDate();
  },
});
</script>
