<script setup lang="ts">
import GameGrid from './components/GameGrid.vue'
</script>

<template>
  <header>
    <h1>Memory Match</h1>
  </header>
  <main>
    <p>Welcome to Memory Match. Open matching tiles to win.</p>
    <GameGrid />
  </main>
  <footer></footer>
</template>

<style scoped>
h1 {
  font-size: 3rem;
}
p {
  padding-bottom: 16px;
}
</style>
