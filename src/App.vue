<template>
  <div class="container">
    <Ring
      v-for="ring in Object.values(rings)"
      :key="ring.index"
      :radius="ring.radius"
      :progress="ring.progress"
      :stroke="ring.stroke"
      :strokeColour="ring.strokeColour"
    />
  </div>
</template>

<script lang="ts">
import Ring from "./components/Ring.vue";

const width = window.innerWidth;
const isSmall = width < 400;

export default {
  components: {
    Ring,
  },
  data() {
    return {
      rings: {
        month: {
          index: 0,
          radius: isSmall ? "220" : "360",
          progress: 0,
          stroke: isSmall ? 35 : 60,
          strokeColour: "#cdb4db",
        },
        day: {
          index: 1,
          radius: isSmall ? "175" : "280",
          progress: 0,
          stroke: isSmall ? 30 : 50,
          strokeColour: "#ffc8dd",
        },
        hours: {
          index: 2,
          radius: isSmall ? "135" : "210",
          progress: 0,
          stroke: isSmall ? 25 : 40,
          strokeColour: "#ffafcc",
        },
        minutes: {
          index: 3,
          radius: isSmall ? "100" : "150",
          progress: 0,
          stroke: isSmall ? 20 : 30,
          strokeColour: "#bde0fe",
        },
        seconds: {
          index: 4,
          radius: isSmall ? "70" : "115",
          progress: 0,
          stroke: isSmall ? 15 : 30,
          strokeColour: "#a2d2ff",
        },
      },
    };
  },
  methods: {
    scale(number: number, inMin: number, inMax: number) {
      return ((number - inMin) * (100 - 0)) / (inMax - inMin);
    },
    zeroPad(number: number) {
      // Pad single digit values with 0
      return `0${Math.floor(number)}`.slice(-2);
    },
  },
  mounted() {
    let dateObj = new Date();

    // Calculate last day of the month to
    // calculate correct scale for ring
    const lastDay = new Date(dateObj.getFullYear(), dateObj.getMonth() + 1, 0);

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

      const minutesWithSeconds = parseFloat(
        `${minutes}.${this.zeroPad(secondsScaled)}`
      );
      const minutesScaled = this.scale(minutesWithSeconds, 0, 61);

      this.rings.seconds.progress = secondsScaled;
      this.rings.minutes.progress = minutesScaled;

      const day = this.scale(dateObj.getDate(), 1, lastDay.getDate());
      const month = this.scale(dateObj.getMonth() + 1, 1, 12);

      // Only render change in hours, days
      // and months when necessary
      if (this.rings.hours.progress != hours)
        this.rings.hours.progress = this.scale(hours, 0, 24);
      if (this.rings.day.progress != day) this.rings.day.progress = day;
      if (this.rings.month.progress != month) this.rings.month.progress = month;
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
  animation: entry 1s forwards cubic-bezier(0.71, 0.1, 0.19, 1.05);
  overflow: hidden;
}

@keyframes entry {
  0% {
    transform: scale(0);
  }
  100% {
    transform: scale(1);
  }
}
</style>
