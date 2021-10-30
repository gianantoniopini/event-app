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
                  <div class="col-12 align-self-end">
                    <button type="submit" class="btn btn-primary">
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
      eventDate: new Date(2021, 11, 17, 18, 0, 0),
      reminderDate: "",
      reminderTime: new Date(),
      reminderDays: 5,
      reminderEmail: "",
    };
  },

  methods: {
    initialize() {
      const now = new Date();
      const reminderDate = new Date(
        now.setDate(now.getDate() + this.reminderDays)
      );

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
      this.reminderDate = `${reminderDate.getFullYear()}-${reminderDateMonthString}-${reminderDateDayString}`;

      this.reminderTime = new Date(reminderDate.getTime());
      this.reminderTime.setHours(13);
      this.reminderTime.setMinutes(0);
    },
  },

  mounted() {
    this.initialize();
  },
});
</script>
