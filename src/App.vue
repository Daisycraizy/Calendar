<template>
  <div id="app">
    <WeeklyCalendar :calendarData="calendarData" @update:calendarData="updateCalendarData" />
  </div>
</template>

<script>
import WeeklyCalendar from './components/WeeklyCalendar.vue';

export default {
  name: 'App',
  components: {
    WeeklyCalendar
  },
  data() {
    return {
      calendarData: {
        mo: [{ bt: 240, et: 779 }],
        tu: [],
        we: [],
        th: [{ bt: 240, et: 779 }, { bt: 1140, et: 1319 }],
        fr: [{ bt: 660, et: 1019 }],
        sa: [{ bt: 0, et: 1439 }],
        su: []
      }
    };
  },
  methods: {
    updateCalendarData(newData) {
      this.calendarData = newData;
    }
  }
};
</script>

<style>
/* Ваши стили */
</style>