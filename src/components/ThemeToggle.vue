<template>
  <svg
    class="toggle"
    @click="onClick"
    width="24"
    height="24"
    fill="grey"
    viewBox="0 0 24 24"
    xmlns="http://www.w3.org/2000/svg"
  >
    <path
      fill-rule="evenodd"
      d="M12 3a9 9 0 0 0 0 18c.83 0 1.5-.67 1.5-1.5 0-.39-.15-.74-.39-1.01-.23-.26-.38-.61-.38-.99 0-.83.67-1.5 1.5-1.5H16c2.76 0 5-2.24 5-5 0-4.42-4.03-8-9-8Zm-5.5 9c-.83 0-1.5-.67-1.5-1.5S5.67 9 6.5 9 8 9.67 8 10.5 7.33 12 6.5 12ZM8 6.5C8 7.33 8.67 8 9.5 8S11 7.33 11 6.5 10.33 5 9.5 5 8 5.67 8 6.5ZM14.5 8c-.83 0-1.5-.67-1.5-1.5S13.67 5 14.5 5s1.5.67 1.5 1.5S15.33 8 14.5 8Zm1.5 2.5c0 .83.67 1.5 1.5 1.5s1.5-.67 1.5-1.5S18.33 9 17.5 9s-1.5.67-1.5 1.5Z"
      clip-rule="evenodd"
    ></path>
  </svg>
</template>

<script lang="ts">
export default {
  name: "ThemeToggle",
  data() {
    return {
      isThemeSelectorActive: false,
    }
  },
  methods: {
    generateRandomColour(){
      return `hsla(${~~(360 * Math.random())},70%,70%,0.8)`
    },
    onClick() {
      // Create an array of size 5 and apply a random
      // colour to each value
      const colours = Array.from(
        Array(5).fill(null), _ => this.generateRandomColour()
      );

      this.$emit("colourChange", colours);
    }
  }
};
</script>

<style scoped>
.toggle {
  position: absolute;
  top: 36px;
  right: 0;
  padding: 10px;
  margin: 10px;
  opacity: 0;
  cursor: pointer;
  animation: entry 1s linear 1s forwards;
}

@keyframes entry {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>