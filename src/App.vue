<script setup lang="ts">
import Taskbar from './components/Taskbar.vue'
</script>

<template>
  <img alt="background" src="/background.jpg" class="background-img" />
  <RouterView />
  <Taskbar />
</template>

<style scoped lang="scss">
:global(#app) {
  height: 100vh;
  width: 100vw;
}

.background-img {
  position: absolute;
  width: 100%;
  height: 100%;
  object-fit: cover;
  z-index: -1;
}
</style>
