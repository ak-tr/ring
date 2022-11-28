<template>
  <div class="container">
    <Ring radius="360" :progress="month" stroke="50" />
    <Ring radius="280" :progress="day" stroke="40" />
    <Ring radius="210" :progress="hours" stroke="30" />
    <Ring radius="150" :progress="minutes" stroke="20" />
    <Ring radius="110" :progress="seconds" stroke="20" />
  </div>
</template>

<script lang="ts">
import Ring from "./components/Ring.vue";

export default {
  components: {
    Ring,
  },
  data() {
    return {
      seconds: 0,
      minutes: 0,
      hours: 0,
      day: 0,
      month: 0,
    };
  },
  methods: {
    scale(
      number: number,
      inMin: number,
      inMax: number,
    ) {
      return ((number - inMin) * (100 - 0)) / (inMax - inMin);
    },
  },
  mounted() {
    let dateObj = new Date();

    // Calculate last day of the month to
    // calcualate correct scale for ring
    const lastDay = new Date(dateObj.getFullYear(), dateObj.getMonth() + 1, 0)
    this.day = this.scale(dateObj.getDate(), 1, lastDay.getDate());

    this.month = this.scale(dateObj.getMonth() + 1, 1, 12);

    setInterval(() => {
      dateObj = new Date();

      const millis = dateObj.getMilliseconds();
      const seconds = dateObj.getSeconds();
      const minutes = dateObj.getMinutes();
      const hours = dateObj.getHours();

      // Parse seconds and milliseconds into float
      const secondsWithMillis = parseFloat(`${seconds}.${millis}`);
      // Map range of 0-60 to range 0-100
      const secondsScaled = this.scale(secondsWithMillis, 0, 60);

      this.seconds = secondsScaled;
      
      // Only update minutes and hours when
      // necessary
      if (this.minutes != minutes)
        this.minutes = minutes;
      if (this.hours != hours)
        this.hours = this.scale(hours, 0, 24);
    }, 500);
  },
};
</script>

<style scoped>
.container {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
