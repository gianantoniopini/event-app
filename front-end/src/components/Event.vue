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
                <div>{{ eventDate }}</div>
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
                      required
                    />
                  </div>
                  <div class="col-2">
                    <input
                      class="form-control text-center"
                      type="time"
                      v-model="reminderTime"
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
import { zonedTimeToUtc } from "date-fns-tz";

export default defineComponent({
  name: "Event",

  data() {
    return {
      eventDate: zonedTimeToUtc("2021-12-17 18:00:00.000", "Europe/Berlin"),
      reminderDate: "",
      reminderTime: "13:00",
      reminderDays: 5,
      reminderEmail: "",
    };
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
      console.log(this.reminderTime);
    },
  },

  mounted() {
    this.reminderDate = this.calculateReminderDate();
  },

  watch: {
    reminderDate() {
      const reminderDays = this.calculateReminderDays();

      if (this.reminderDays === reminderDays) {
        return;
      }

      this.reminderDays = reminderDays;
    },

    reminderDays() {
      const reminderDate = this.calculateReminderDate();

      if (this.reminderDate === reminderDate) {
        return;
      }

      this.reminderDate = reminderDate;
    },
  },
});
</script>
