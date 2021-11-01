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

export default defineComponent({
  name: "Event",

  data() {
    return {
      eventDate: new Date(2021, 11, 17, 18, 0, 0, 0),
      now: new Date(),
      reminderDate: "",
      reminderTime: "13:00",
      reminderDays: 5,
      reminderEmail: "",
    };
  },

  methods: {
    calculateReminderDate() {
      const reminderDate = new Date(this.now.getTime());

      reminderDate.setDate(reminderDate.getDate() + this.reminderDays);

      // we need to build a date string with format YYYY-MM-DD in order for binding to input with type="date" to work
      // TODO: consider moving this to separate method, or use external package
      const reminderDateMonth = reminderDate.getMonth() + 1;
      const reminderDateMonthString =
        reminderDateMonth < 10
          ? `0${reminderDateMonth}`
          : reminderDateMonth.toString();
      const reminderDateDay = reminderDate.getDate();
      const reminderDateDayString =
        reminderDateDay < 10
          ? `0${reminderDateDay}`
          : reminderDateDay.toString();

      return `${reminderDate.getFullYear()}-${reminderDateMonthString}-${reminderDateDayString}`;
    },

    calculateReminderDays() {
      const fullYear = parseInt(this.reminderDate.substring(0, 4));
      const month = parseInt(this.reminderDate.substring(5, 7)) - 1;
      const day = parseInt(this.reminderDate.substring(8));
      const reminderDate = new Date(fullYear, month, day, 0, 0, 0, 0);

      const reminderTime = Math.abs(
        reminderDate.getTime() - this.now.getTime()
      );
      const reminderDays = Math.ceil(reminderTime / (1000 * 60 * 60 * 24));

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
