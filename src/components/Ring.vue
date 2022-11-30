<template>
    <svg
    class="ring"
    :style="{ transform }"
    :height="radius * 2"
    :width="radius * 2"
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
  props: ["radius", "progress", "stroke", "strokeColour", "scale"],
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
    },
    transform() {
      return `scale(${this.scale})`;
    }
  }
}
</script>

<style scoped>
.ring {
  position: absolute;
  transition: transform 0.25s cubic-bezier(0.31, 0.03, 0, 1);
}

.main-circle {
  filter: drop-shadow(5px 5px 24px rgba(255, 255, 255, 0.15));
  stroke-linecap: round;
  transition: 
    stroke-dashoffset 1s cubic-bezier(0.31, 0.03, 0, 1), 
    stroke 0.25s cubic-bezier(0.31, 0.03, 0, 1);
}

.underlying-circle {
  opacity: 0.2;
}
</style>