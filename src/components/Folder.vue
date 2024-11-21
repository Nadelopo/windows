<script setup lang="ts">
defineProps<{
  name: string
}>()
</script>

<template>
  <div class="folder">
    <img src="/icons/folder.svg" />
    <div class="text">
      <div>{{ name }} + asdas ddsadas asd asddadsads</div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.folder {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  color: #fff;
  border-radius: 2px;
  position: relative;
  &:hover {
    background: #d1d1d147;
  }
  img {
    user-select: none;
  }
  .text {
    position: absolute;
    bottom: 0;
    left: 0;
    // right: 0;
    // padding: 0 10px;
    text-align: center;
  }
}
</style>
