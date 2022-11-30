<template>
  <div class="footer">
    made with ♥ by akif - <a href="https://git.akif.kr/rings">github</a>
  </div>
</template>

<script lang="ts">
export default {
  name: "AppFooter",
}
</script>

<style>
@import url('https://fonts.cdnfonts.com/css/sf-mono');

.footer {
  font-family: "SF Mono", sans-serif;
  position: absolute;
  font-size: 12px;
  bottom: 0;
  padding-bottom: 10px;
  color: grey;
  opacity: 0;
  animation: entry 1s linear 1s forwards;
}

a {
  color: grey;
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