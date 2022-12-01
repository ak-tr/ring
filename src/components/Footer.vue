<template>
  <div class="footer">
    source code on <a href="https://git.akif.kr/rings">github</a><br>made with <span class="rotate">♥</span> by akif
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
  right: 0;
  bottom: 0;
  padding: 0 10px 15px 0;
  color: grey;
  opacity: 0;
  animation: entry 1s linear 1s forwards;
  writing-mode: vertical-lr;
  user-select: none;
  line-height: 16px;
  text-align: center;
}

a {
  color: grey;
}

.rotate {
  display: inline-block;
  transform: rotate(90deg);
  -webkit-transform: rotate(90deg);
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