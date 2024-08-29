<script setup lang="ts">
import { ref, reactive, computed } from 'vue'

type GridItem = {
  id: number
  value: number
  isActive: boolean
  isOpen: boolean
}
const colorMap = [
  '#FF5733', // Red-Orange
  '#33FF57', // Green
  '#3357FF', // Blue
  '#FF33A1', // Pink
  '#33FFF5', // Cyan
  '#F5FF33', // Yellow
  '#FF8C33', // Orange
  '#8C33FF' // Purple
]
const turns = ref(0)

const initialItems = []
for (let i = 1; i < 17; i++) {
  initialItems.push({
    id: i,
    value: Math.floor((i + 1) / 2),
    isActive: false,
    isOpen: false
  })
}

// randomize order
initialItems.sort(() => Math.random() - 0.5)

const getBackgroundColor = (item: GridItem) => {
  return item.isOpen ? colorMap[item.value - 1] : 'lightblue'
}

const gridItems = reactive(initialItems)

const activeItems = computed(() => gridItems.filter((item) => item.isActive))

const isWin = computed(() => gridItems.every((i) => i.isOpen))

const resetGame = () => {
  gridItems.forEach((i) => (i.isActive = false))
  gridItems.forEach((i) => (i.isOpen = false))

  // randomize order
  gridItems.sort(() => Math.random() - 0.5)
  turns.value = 0
}

const closeAfterDelay = (items: GridItem[], delay: number) => {
  setTimeout(() => {
    items.forEach((i) => (i.isOpen = false))
    items.forEach((i) => (i.isActive = false))
  }, delay)
}

const handleClick = (item: GridItem) => {
  if (activeItems.value.length >= 2 || isWin.value || item.isOpen) {
    return
  }

  item.isActive = true
  item.isOpen = true

  if (activeItems.value.length === 2) {
    turns.value++
    if (activeItems.value.every((i) => i.value === item.value)) {
      // register win
      activeItems.value.forEach((i) => (i.isActive = false))
    } else {
      // register loss
      closeAfterDelay(activeItems.value, 1200)
    }
  }
}
</script>

<template>
  <button class="reset-button" @click="resetGame">Reset</button>
  <div class="grid-wrapper">
    <button
      :class="{ open: item.isOpen }"
      class="grid-item"
      v-for="item in gridItems"
      :key="item.id"
      @click="handleClick(item)"
      :style="{ backgroundColor: getBackgroundColor(item) }"
    ></button>
  </div>
  <div>You have used {{ turns }} turns</div>
  <div v-if="isWin">You win!</div>
</template>

<style scoped>
.grid-wrapper {
  display: grid;
  gap: 1rem;
  border-radius: 5%;
  padding-top: 16px;
  padding-bottom: 16px;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(4, 1fr);
}

.grid-item {
  aspect-ratio: 1;
  background-color: rgb(59, 206, 201);
  color: black;
  border-radius: 5%;
  cursor: pointer;
  font-size: xx-large;
  transition: 1s;
  border: 0;
}

.open {
  transform: rotateY(180deg);
}

.reset-button {
  background: #5e5df0;
  border-radius: 999px;
  box-shadow: #5e5df0 0 10px 20px -10px;
  box-sizing: border-box;
  color: #ffffff;
  cursor: pointer;
  font-family:
    Inter,
    Helvetica,
    'Apple Color Emoji',
    'Segoe UI Emoji',
    NotoColorEmoji,
    'Noto Color Emoji',
    'Segoe UI Symbol',
    'Android Emoji',
    EmojiSymbols,
    -apple-system,
    system-ui,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    'Noto Sans',
    sans-serif;
  font-size: 16px;
  font-weight: 700;
  line-height: 24px;
  opacity: 1;
  outline: 0 solid transparent;
  padding: 8px 18px;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  width: fit-content;
  word-break: break-word;
  border: 0;
}
</style>
