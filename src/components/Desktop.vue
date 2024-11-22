<script setup lang="ts">
import { computed, reactive, ref } from 'vue'
import Folder from './Folder.vue'

type FileItem = {
  id: number
  name: string
  row: number
  col: number
}

type GridParams = Record<keyof typeof gridCssParams, number> & {}

const gridCssParams = reactive({
  cellSize: '80px',
  rows: 0,
  cols: 0,
  gap: '10px',
})

const gridParams = computed(
  () =>
    Object.fromEntries(
      Object.entries(gridCssParams).map(([key, value]) => [key, parseInt(value as string)]),
    ) as GridParams,
)

gridCssParams.rows = Math.floor(
  window.innerHeight / (gridParams.value.cellSize + gridParams.value.gap),
)
gridCssParams.cols = Math.floor(
  window.innerWidth / (gridParams.value.cellSize + gridParams.value.gap),
)

console.log(gridParams)

// Список файлов/папок
const files = ref<FileItem[]>([
  { id: 1, name: 'Документ  as dassad  dasdadasd', row: 1, col: 5 },
  { id: 2, name: 'Папка 1', row: 2, col: 2 },
])

// Функция для привязки к ближайшей ячейке
// const snapToGrid = (x: number, y: number) => {
//   const col = Math.max(
//     1,
//     Math.min(gridParams.value.cols, Math.round(x / gridParams.value.cellSize) + 1),
//   )
//   const row = Math.max(
//     1,
//     Math.min(gridParams.value.rows, Math.round(y / gridParams.value.cellSize) + 1),
//   )
//   return { row, col }
// }

// Обновление позиции элемента
// const updatePosition = (id: number, x: number, y: number) => {
//   const { row, col } = snapToGrid(x, y)
//   const file = files.value.find((f) => f.id === id)
//   if (file) {
//     file.row = row
//     file.col = col
//   }
// }
</script>

<template>
  <div class="desktop">
    <Folder
      v-for="file in files"
      :key="file.id"
      :style="{
        gridRow: file.row,
        gridColumn: file.col,
      }"
      :name="file.name"
    />
  </div>
</template>

<style scoped>
.desktop {
  display: grid;
  grid-template-rows: repeat(v-bind('gridCssParams.rows'), v-bind('gridCssParams.cellSize'));
  grid-template-columns: repeat(v-bind('gridCssParams.cols'), v-bind('gridCssParams.cellSize'));
  gap: 10px;
  width: 100%;
  height: 100%;
  padding: 10px;
}

.file-item {
  background: #0078d7;
  color: white;
  text-align: center;
  line-height: 90px;
  border-radius: 4px;
  user-select: none;
  cursor: grab;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.2);
}

.file-item:active {
  cursor: grabbing;
}
</style>
