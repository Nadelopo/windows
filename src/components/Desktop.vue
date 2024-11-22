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

const files = ref<FileItem[]>([
  { id: 1, name: 'Документ  as dassad  dasdadasd', row: 1, col: 5 },
  { id: 2, name: 'Папка 1', row: 2, col: 2 },
])

const onDrop = (event: DragEvent, file: FileItem) => {
  const { cellSize, gap, rows, cols } = gridParams.value
  const row = Math.ceil(event.y / (cellSize + gap))
  const col = Math.ceil(event.x / (cellSize + gap))

  if (row > rows || col > cols) return

  const isCellOccupied = files.value.some((f) => f.row === row && f.col === col && f.id !== file.id)
  if (isCellOccupied) return

  file.row = row
  file.col = col
}
</script>

<template>
  <div class="desktop" @dragover.prevent>
    <Folder
      v-for="file in files"
      :key="file.id"
      v-model="file.name"
      :style="{ gridRow: file.row, gridColumn: file.col }"
      @dragend="onDrop($event, file)"
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

.dragging {
  opacity: 0.99;
  background: none;
  color: transparent;
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
