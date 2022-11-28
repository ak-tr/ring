<template>
  <div class="container">
    <Ring radius="360" :progress="month" stroke="50" strokeColour="#cdb4db" />
    <Ring radius="280" :progress="day" stroke="40" strokeColour="#ffc8dd" />
    <Ring radius="210" :progress="hours" stroke="30" strokeColour="#ffafcc" />
    <Ring radius="150" :progress="minutes" stroke="20" strokeColour="#bde0fe" />
    <Ring radius="115" :progress="seconds" stroke="20" strokeColour="#a2d2ff" />
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
    zeroPad(number: number) {
      // Pad single digit values with 0
      return `0${Math.floor(number)}`.slice(-2);
    }
  },
  mounted() {
    let dateObj = new Date();

    // Calculate last day of the month to
    // calculate correct scale for ring
    const lastDay = new Date(dateObj.getFullYear(), dateObj.getMonth() + 1, 0)

    this.day = this.scale(dateObj.getDate(), 1, lastDay.getDate());
    this.month = this.scale(dateObj.getMonth() + 1, 1, 12);

    // 1 second loop
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

      const minutesWithSeconds = parseFloat(`${minutes}.${this.zeroPad(secondsScaled)}`)
      const minutesScaled = this.scale(minutesWithSeconds, 0, 61);

      this.seconds = secondsScaled;
      this.minutes = minutesScaled;
      
      // Only render change in hours when necessary
      if (this.hours != hours)
        this.hours = this.scale(hours, 0, 24);
    }, 1000);
  },
};
</script>

<style scoped>
.container {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  animation: entry 1s forwards cubic-bezier(.71,.1,.19,1.05);
}

@keyframes entry {
  0% { transform: scale(0); }
  100% { transform: scale(1); }
}
</style>
