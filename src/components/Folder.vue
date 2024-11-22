<script setup lang="ts">
import { ref, useTemplateRef, watch, watchEffect } from 'vue'
import { useTextareaAutosize } from '@vueuse/core'
import { onClickOutsideClose } from '@/shared/composables/onClickOutsideClose'

const props = defineProps<{
  name: string
}>()

const folderRef = useTemplateRef('folder')
const active = onClickOutsideClose(folderRef)

const isInputMode = ref(false)

watchEffect(() => {
  if (!active.value) {
    isInputMode.value = false
  }
})

const { textarea, input } = useTextareaAutosize({ input: props.name })
watchEffect(() => {
  textarea.value?.select()
})

let previousFolderName = ''
watch(isInputMode, () => {
  previousFolderName = input.value
})
</script>

<template>
  <div ref="folder" class="folder" @click="active = true">
    <div class="content" tabindex="0" :class="{ active }">
      <img src="/icons/folder.svg" />
      <textarea
        v-if="isInputMode"
        ref="textarea"
        v-model="input"
        v-focus
        @keydown.escape="(input = previousFolderName), (active = false)"
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
  &:hover {
    background: $bg;
  }
  &.active {
    background: $bg;
  }
  img {
    user-select: none;
  }
  .text {
    width: 100%;
    padding: 2px;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
    word-wrap: break-word;
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
  }
}
</style>
