<script setup>
import { GiphyFetch } from '@giphy/js-fetch-api'
import { ref, onMounted, provide } from 'vue'

import Player from './components/Player.vue'

const gifs = ref([])
const currentGif = ref()

const gf = new GiphyFetch(import.meta.env.VITE_GIPHY_KEY)

const retrieveGifObjects = async () => {
  const { data: gifObjects } = await gf.search('lofi', {
    sort: 'relevant',
    limit: 6,
    type: 'gifs',
  })
  return gifObjects
}

const retrieveGifs = (arrayOfObjects) => {
  const array = arrayOfObjects
    .map((gif) => gif.id)
    .map((gifId) => `https://media.giphy.com/media/${gifId}/giphy.gif`)
  return array
}

const changeCurrentGif = () => {
  const newGif = gifs.value[Math.floor(Math.random() * gifs.value.length)]
  currentGif.value = newGif
}

onMounted(async () => {
  try {
    const gifObjects = await retrieveGifObjects()
    gifs.value = retrieveGifs(gifObjects)
    changeCurrentGif()
  } catch (error) {
    console.error(error)
  }
})

provide('gif', {
  changeCurrentGif,
})
</script>

<template>
  <main id="container">
    <header>
      <h1>lofi town.</h1>
      <img
        id="giphy-logo"
        src="./assets/Poweredby_100px-White_VertLogo.png"
        alt="Powered By GIPHY logo"
      />
    </header>
    <Player></Player>
  </main>
  <img id="background" :src="currentGif" alt="GIF image of a lofi scene" />
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

#background {
  z-index: -1;
  position: absolute;
  width: 100vw;
  height: 100vh;
  top: 0px;
  left: 0px;
  object-fit: cover;
  filter: brightness(50%);
}

header {
  display: grid;
  grid-template-columns: 1fr auto;
  width: 100%;
}
</style>
