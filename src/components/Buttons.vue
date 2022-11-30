<template>
  <div class="buttons">
    <ModeToggle @mode-toggled="onModeToggled" />
    <ThemeRandomiser @colour-change="onColourChangeRequest" />
    <DefaultTheme @colour-change="onColourChangeRequest" />
  </div>
</template>

<script lang="ts">
import ModeToggle from './buttons/ModeToggle.vue';
import ThemeRandomiser from './buttons/ThemeRandomiser.vue';
import DefaultTheme from './buttons/DefaultTheme.vue';

export default {
  name: "AppButtons",
  components: {
    ModeToggle,
    ThemeRandomiser,
    DefaultTheme,
  },
  methods: {
    // Functions to re-emit events back up to parent
    onModeToggled(isDarkMode: boolean) {
      this.$emit("modeToggled", isDarkMode);
    },
    onColourChangeRequest(colours: string[]) {
      this.$emit("colourChange", colours);
    }
  }
}
</script>

<style>
.buttons {
  display: flex;
  flex-direction: column;
  gap: 15px;
  margin: 15px;
  position: absolute;
  top: 0;
  right: 0;
}
</style>