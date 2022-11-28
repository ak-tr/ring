<template>
    <svg
    class="ring"
    :height="radius * 2"
    :width="radius * 2"
    >
      <circle
        class="underlying-circle"
        stroke="grey"
        fill="transparent"
        :stroke-dasharray="circumference + ' ' + circumference"
        :stroke-width="stroke"
        :r="normalizedRadius"
        :cx="radius"
        :cy="radius"
        />
      <circle
        stroke="white"
        fill="transparent"
        :stroke-dasharray="circumference + ' ' + circumference"
        :style="{ strokeDashoffset }"
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
  props: ["radius", "progress", "stroke"],
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

circle {
  transition: stroke-dashoffset 0.5s ease-in;
}

/* 
.ring:hover {
  opacity: 0.2;
} */

.underlying-circle {
  opacity: 0.2;
}
</style>