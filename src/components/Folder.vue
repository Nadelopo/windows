<script setup lang="ts">
import { ref, useTemplateRef, watch, watchEffect } from 'vue'
import { useTextareaAutosize } from '@vueuse/core'
import { onClickOutsideClose } from '@/shared/composables/onClickOutsideClose'

const name = defineModel<string>({ required: true })

const emit = defineEmits<{
  dragend: [DragEvent]
}>()

const folderRef = useTemplateRef('folder')
const active = onClickOutsideClose(folderRef)

const isInputMode = ref(false)

watchEffect(() => {
  if (!active.value) {
    isInputMode.value = false
  }
})

const { textarea } = useTextareaAutosize({ input: name })
watchEffect(() => {
  textarea.value?.select()
})

let previousFolderName = ''
watch(isInputMode, () => {
  previousFolderName = name.value
})

const onEscape = () => {
  name.value = previousFolderName
  active.value = false
}

const isDragging = ref(false)
</script>

<template>
  <div
    ref="folder"
    class="folder"
    :class="{ dragging: isDragging }"
    draggable="true"
    @click="active = true"
    @dragstart="isDragging = true"
    @dragend.prevent="emit('dragend', $event), (isDragging = false)"
  >
    <div class="content" tabindex="0" :class="{ active }">
      <img src="/icons/folder.svg" draggable="false" />
      <textarea
        v-if="isInputMode"
        ref="textarea"
        v-model="name"
        v-focus
        @keydown.escape="onEscape"
        @keydown.enter="active = false"
      />
      <div v-else class="text" :class="{ active }" @click="isInputMode = true">
        {{ name }}
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
$bg: #d1d1d147;

.folder {
  position: relative;
  &.dragging {
    opacity: 0.99;
    background: none;
    color: transparent;
  }
}
.content {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  color: #fff;
  border-radius: 2px;
  height: 100%;
  width: 100%;
  height: auto;
  position: absolute;
  outline: none;
  font-size: 12px;
  &:hover {
    background: $bg;
  }
  &.active {
    background: $bg;
  }
  .text {
    width: 100%;
    padding: 2px;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
    word-wrap: break-word;
    user-select: none;
    text-align: center;
    &.active {
      overflow: unset;
      white-space: unset;
      text-overflow: unset;
    }
  }
  textarea {
    width: 100%;
    resize: none;
    color: #000;
    overflow-y: hidden;
    padding: 2px;
    outline: none;
    text-align: center;
  }
}
</style>
