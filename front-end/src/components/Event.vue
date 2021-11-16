<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-sm-12 col-lg-8 offset-lg-2">
        <EventDetails
          v-bind:eventDateTime="this.eventDateTime"
          v-bind:eventTimeZone="this.eventTimeZone"
        />
        <div class="row mt-2 mt-md-5">
          <div class="col-12">
            <hr />
          </div>
        </div>
        <div class="row mt-md-5">
          <form @submit.prevent="onSubmit" class="col-12 form">
            <div class="row">
              <div class="col-12 fw-bold">Set a check back reminder</div>
            </div>
            <div class="row">
              <div class="col-8 col-md-4 mt-2">
                <input
                  class="form-control"
                  type="date"
                  v-model="reminderDate"
                  :class="{
                    'is-invalid': this.validationErrors.reminderDateTime,
                  }"
                  data-toggle="tooltip"
                  :title="this.validationErrors.reminderDateTime"
                  required
                  aria-label="Reminder date"
                />
                <div class="invalid-feedback">
                  {{ this.validationErrors.reminderDateTime }}
                </div>
              </div>
              <div class="col-4 col-md-3 mt-2">
                <input
                  class="form-control text-center"
                  type="time"
                  v-model="reminderTime"
                  :class="{
                    'is-invalid': this.validationErrors.reminderDateTime,
                  }"
                  data-toggle="tooltip"
                  :title="this.validationErrors.reminderDateTime"
                  required
                  aria-label="Reminder time"
                />
              </div>
              <div class="col-12 col-md-5 mt-2">
                <div class="hstack gap-3">
                  <div>in</div>
                  <input
                    class="form-control text-center"
                    type="number"
                    v-model="reminderDays"
                    required
                    aria-label="Reminder days"
                  />
                  <div>Days</div>
                </div>
              </div>
            </div>
            <div class="row mt-2">
              <div class="col-12">
                <input
                  class="form-control"
                  type="text"
                  v-model="reminderEmail"
                  :class="{
                    'is-invalid': this.validationErrors.reminderEmail,
                  }"
                  data-toggle="tooltip"
                  :title="this.validationErrors.reminderEmail"
                  placeholder="Enter your email"
                  aria-label="Email"
                />
                <div class="invalid-feedback">
                  {{ this.validationErrors.reminderEmail }}
                </div>
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
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { format } from "date-fns";
import add from "date-fns/add";
import parseISO from "date-fns/parseISO";
import differenceInCalendarDays from "date-fns/differenceInCalendarDays";
import { zonedTimeToUtc } from "date-fns-tz";
import EventDetails from "./EventDetails.vue";

export default defineComponent({
  name: "Event",

  components: {
    EventDetails,
  },

  data() {
    return {
      eventDateTime: new Date(),
      eventTimeZone: "",
      reminderDate: "",
      reminderTime: "13:00",
      reminderDays: 5,
      reminderEmail: "",
      validationErrors: {
        reminderDateTime: "",
        reminderEmail: "",
      },
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

    reminderEmail() {
      this.validateReminderEmail();
    },
  },

  computed: {
    invalid(): boolean {
      return (
        this.validationErrors.reminderDateTime.length > 0 ||
        this.validationErrors.reminderEmail.length > 0
      );
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
      this.validateReminderEmail();

      if (this.invalid) {
        return;
      }

      console.log("submitting form values...");
    },

    setEventDateTime() {
      this.eventTimeZone = "Europe/Berlin";
      this.eventDateTime = zonedTimeToUtc(
        "2021-12-17 18:00:00.000",
        this.eventTimeZone
      );
    },

    validateReminderDateTime() {
      const reminderDateTime = parseISO(this.reminderDate);
      const reminderHours = parseInt(this.reminderTime.split(":")[0]);
      const reminderMinutes = parseInt(this.reminderTime.split(":")[1]);
      reminderDateTime.setHours(reminderHours);
      reminderDateTime.setMinutes(reminderMinutes);

      this.validationErrors.reminderDateTime = "";

      if (reminderDateTime > this.eventDateTime) {
        this.validationErrors.reminderDateTime =
          "The reminder date cannot be past the event date";
      } else if (reminderDateTime <= new Date()) {
        this.validationErrors.reminderDateTime =
          "The reminder date cannot be in the past";
      }
    },

    validateReminderEmail() {
      this.validationErrors.reminderEmail = "";

      if (!this.reminderEmail) {
        this.validationErrors.reminderEmail = "Email must be provided";
      } else {
        const regExp = new RegExp(
          /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
        );
        if (!regExp.test(this.reminderEmail)) {
          this.validationErrors.reminderEmail = "Please enter a valid email";
        }
      }
    },
  },

  mounted() {
    this.setEventDateTime();
    this.reminderDate = this.calculateReminderDate();
  },
});
</script>
