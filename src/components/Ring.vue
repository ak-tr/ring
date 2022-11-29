<template>
    <svg
    class="ring"
    :height="radius * 2"
    :width="radius * 2"
    ref="ring"
    >
      <circle
        class="main-circle"
        :stroke="strokeColour"
        fill="transparent"
        :stroke-dasharray="`${circumference} ${circumference}`"
        :style="{ strokeDashoffset }"
        :stroke-width="stroke"
        :r="normalizedRadius"
        :cx="radius"
        :cy="radius"
      />
      <circle
        class="underlying-circle"
        stroke="grey"
        fill="transparent"
        :stroke-dasharray="`${circumference} ${circumference}`"
        :stroke-width="stroke"
        :r="normalizedRadius"
        :cx="radius"
        :cy="radius"
      />
    </svg>
</template>

<script lang="ts">
export default {
  name: "Ring",
  props: ["radius", "progress", "stroke", "strokeColour"],
  data() {
    const normalizedRadius = this.radius - this.stroke * 2;
    const circumference = normalizedRadius * 2 * Math.PI;

    return {
      normalizedRadius,
      circumference
    };
  },
  computed: {
    strokeDashoffset() {
      return this.circumference - this.progress / 100 * this.circumference;
    }
  }
}
</script>

<style scoped>
.ring {
  position: absolute;
}

.main-circle {
  filter: drop-shadow(5px 5px 24px rgba(255, 255, 255, 0.15));
  stroke-linecap: round;
  transition: stroke-dashoffset 1s linear;
}

.underlying-circle {
  opacity: 0.2;
}
</style>