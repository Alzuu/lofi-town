<script setup>
import { ref, onMounted, computed } from 'vue'

import Controls from './Controls.vue'

const state = ref({
  player: null,
  videoTitle: 'loading...',
})

function initYoutube() {
  state.value.player = new YT.Player('player', {
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
  state.value.player.loadPlaylist({
    list: 'PL6ithAzFBH9bfmzl_2rjN_iZ2GBxoQNsU',
    listType: 'playlist',
  })
  event.target.setLoop(true)
}

function onPlayerStateChange(event) {
  state.value.player = event.target
  state.value.videoTitle = event.target.videoTitle
}

const videoTitle = computed(() => {
  return state.value.videoTitle
})

onMounted(() => {
  // This code loads the IFrame Player API code asynchronously.
  let tag = document.createElement('script')

  tag.src = 'https://www.youtube.com/iframe_api'
  let firstScriptTag = document.getElementsByTagName('script')[0]
  firstScriptTag.parentNode.insertBefore(tag, firstScriptTag)

  window.onYouTubeIframeAPIReady = () => {
    initYoutube()
  }
})
</script>

<template>
  <section id="controls-wrapper">
    <!-- <h1>{{ videoTitle }}</h1> -->
    <Controls :player="state.player" :title="videoTitle"></Controls>
  </section>
  <div id="yt-wrapper" aria-hidden="true">
    <div id="player"></div>
  </div>
</template>

<style scoped>
#yt-wrapper {
  position: fixed;
  top: 100%;
  left: 100%;
  pointer-events: none;
  user-select: none;
}

#controls-wrapper {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  width: 100%;
}

h1 {
  text-align: center;
  max-width: 65vw;
  font-weight: 500;
}
</style>
