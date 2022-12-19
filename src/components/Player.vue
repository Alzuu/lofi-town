<script setup>
import { onMounted, computed } from 'vue'

import Controls from './Controls.vue'

const state = $ref({
  player: null,
  videoTitle: 'loading...',
})

function initYoutube() {
  console.log('initYoutube')
  state.player = new YT.Player('player', {
    playerVars: {
      autoplay: 0,
      playsInline: 1,
      enablesjsapi: 1,
      disablekb: 1,
      controls: 0,
    },
    events: {
      onReady: onPlayerReady,
      onStateChange: onPlayerStateChange,
    },
  })
}

function onPlayerReady(event) {
  console.log('player ready')
  state.player.loadPlaylist({
    list: 'PL6ithAzFBH9bfmzl_2rjN_iZ2GBxoQNsU',
    listType: 'playlist',
  })
  event.target.setLoop(true)
}

function onPlayerStateChange(event) {
  console.log('player state changed')
  state.player = event.target
  state.videoTitle = event.target.videoTitle
}

const videoTitle = computed(() => {
  return state.videoTitle
})

onMounted(() => {
  console.log('onMounted')
  // This code loads the IFrame Player API code asynchronously.
  let tag = document.createElement('script')

  tag.src = 'https://www.youtube.com/iframe_api'
  let firstScriptTag = document.getElementsByTagName('script')[0]
  firstScriptTag.parentNode.insertBefore(tag, firstScriptTag)

  window.onYouTubeIframeAPIReady = () => {
    console.log('onYouTubeIframeAPIReady')
    initYoutube()
  }
})
</script>

<template>
  <div id="yt-wrapper">
    <div id="player"></div>
  </div>
  <h1>{{ videoTitle }}</h1>
  <Controls :player="state.player"></Controls>
</template>

<style scoped>
#yt-wrapper {
  position: fixed;
  top: 100%;
  left: 100%;
  pointer-events: none;
  user-select: none;
}
</style>
