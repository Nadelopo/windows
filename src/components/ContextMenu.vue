<script setup lang="ts">
import { ref, onMounted, onUnmounted, reactive } from 'vue'
import { onClickOutsideClose } from '@/shared/composables/onClickOutsideClose'

const contextMenuRef = ref<HTMLElement>()
const isContextOpen = onClickOutsideClose(contextMenuRef)

const contextWidth = '320px'
const contextHeight = '300px'

const position = reactive({ x: 0, y: 0 })

const onContextMenu = (event: MouseEvent) => {
  event.preventDefault()

  isContextOpen.value = true
  position.x = event.x
  position.y = event.y

  const width = parseInt(contextWidth)
  const height = parseInt(contextWidth)

  if (event.x + width > window.innerWidth) {
    position.x = event.x - width
  }
  if (event.y + height > window.innerHeight) {
    position.y = event.y - height
  }
}

onMounted(() => {
  window.addEventListener('contextmenu', onContextMenu)
})

onUnmounted(() => {
  window.removeEventListener('contextmenu', onContextMenu)
})
</script>

<template>
  <div
    v-if="isContextOpen"
    ref="contextMenuRef"
    class="context-menu"
    :style="{ left: `${position.x}px`, top: `${position.y}px` }"
    @click.right.stop.prevent
  >
    context
  </div>
</template>

<style scoped lang="scss">
.context-menu {
  width: v-bind(contextWidth);
  height: v-bind(contextHeight);
  position: absolute;
  padding: 10px;
  background: rgb(0 0 0 / 36%);
  backdrop-filter: blur(16px);
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
  color: #ffffff;
}
</style>
