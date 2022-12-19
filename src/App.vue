<script setup>
import Player from './components/Player.vue'
</script>

<template>
  <main id="container">
    <h1>lofi town.</h1>
    <Player></Player>
  </main>
</template>

<style scoped>
h1 {
  font-weight: 600;
}

#container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  padding: 2.5rem;
}
</style>
