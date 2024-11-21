<script setup lang="ts">
import { useElementHover, useMouse } from '@vueuse/core'
import { computed, useTemplateRef } from 'vue'

const { y } = useMouse()

const componentRef = useTemplateRef('component')
const isComponentHover = useElementHover(componentRef)

const isOpen = computed(() => window.innerHeight - y.value < 40 || isComponentHover.value)
</script>

<template>
  <div ref="component" class="taskbar" :class="{ open: isOpen }">
    <div class="top-line"></div>
  </div>
</template>

<style scoped lang="scss">
.taskbar {
  position: absolute;
  width: 100%;
  height: 0;
  background: #212121;
  bottom: 0;
  transition: 0.3s;
  overflow: hidden;
  &.open {
    height: 50px;
  }

  .top-line {
    position: relative;
    width: 100%;
    height: 2px;
    background: #424242;
  }
}
</style>
