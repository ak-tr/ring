<template>
  <div class="container">
    <ModeToggle @mode-toggled="onModeToggled" />
    <ThemeToggle @colour-change="onColourChangeRequest" />
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
import ModeToggle from "./components/ModeToggle.vue"
import ThemeToggle from "./components/ThemeToggle.vue";

const strokeColours = ["ffcdb2","ffb4a2","e5989b","b5838d","6d6875"];

// If pixel width of window is less than 740
// device is considered small and rings
// are rendered at a smaller size
const isSmall = window.innerWidth < 740;

export default {
  components: {
    Ring,
    ModeToggle,
    ThemeToggle
  },
  data() {
    return {
      rings: {
        month: {
          index: 0,
          radius: isSmall ? "220" : "360",
          progress: 0,
          stroke: isSmall ? 35 : 60,
          strokeColour: `#${strokeColours[0]}`,
        },
        day: {
          index: 1,
          radius: isSmall ? "175" : "280",
          progress: 0,
          stroke: isSmall ? 30 : 50,
          strokeColour: `#${strokeColours[1]}`,
        },
        hours: {
          index: 2,
          radius: isSmall ? "135" : "210",
          progress: 0,
          stroke: isSmall ? 25 : 40,
          strokeColour: `#${strokeColours[2]}`,
        },
        minutes: {
          index: 3,
          radius: isSmall ? "100" : "150",
          progress: 0,
          stroke: isSmall ? 20 : 30,
          strokeColour: `#${strokeColours[3]}`,
        },
        seconds: {
          index: 4,
          radius: isSmall ? "85" : "115",
          progress: 0,
          stroke: isSmall ? 25 : 30,
          strokeColour: `#${strokeColours[4]}`,
        },
      },
    };
  },
  methods: {
    // Function to map a value from a specified range to
    // a range between 0 and 100
    scale(number: number, inMin: number, inMax: number) {
      return ((number - inMin) * (100 - 0)) / (inMax - inMin);
    },
    // Pad single digit values with 0
    zeroPad(number: number) {
      return `0${Math.floor(number)}`.slice(-2);
    },
    onModeToggled(isDarkMode: boolean) {
      document.body.style.backgroundColor = isDarkMode ? "#171717" : "#f6f9fa";
    },
    onColourChangeRequest(colours: string[]) {
      Object.values(this.rings).forEach((ring, index) => {
        ring.strokeColour = colours[index];
      })
    }
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
  position: relative;
  height: 100vh;
  max-height: -webkit-fill-available;
  display: flex;
  justify-content: center;
  align-items: center;
  animation: entry 1s backwards cubic-bezier(0.16, 1, 0.3, 1);
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
