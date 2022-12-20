<script setup>
import { ref, computed, onMounted } from 'vue'

import { Icon } from '@iconify/vue'

const props = defineProps(['player'])

const volume = ref(100)
const isPlaying = ref(false)

function changePlayback() {
  if (isPlaying.value) {
    console.log('pause')
    props.player.pauseVideo()
  } else {
    console.log('play')
    props.player.playVideo()
  }
  isPlaying.value = !isPlaying.value
}

function previous() {
  console.log('previous video')
  props.player.previousVideo()
  isPlaying.value = true
}

function next() {
  console.log('next video')
  props.player.nextVideo()
  isPlaying.value = true
}

function changeVolume(currentVolume) {
  console.log('change volume to ' + currentVolume)
  props.player.setVolume(currentVolume)
}

function toggleMuted() {
  const playerVolume = props.player.getVolume()
  if (volume.value == 0) {
    console.log('unmuted')
    props.player.unMute()
    volume.value = playerVolume > 0 ? playerVolume : 10
  } else {
    console.log('muted')
    props.player.mute()
    volume.value = 0
  }
}

const playingIcon = computed(() => {
  return isPlaying.value ? 'ion:pause-circle' : 'ion:play-circle'
})

const volumeIcon = computed(() => {
  const currentVolume = volume.value

  if (currentVolume == 0) {
    return 'ion:volume-mute'
  } else if (currentVolume > 75) {
    return 'ion:volume-high'
  } else if (currentVolume > 50) {
    return 'ion:volume-medium'
  } else if (currentVolume > 25) {
    return 'ion:volume-low'
  } else return 'ion:volume-off'
})
</script>

<template>
  <div class="controls">
    <div class="playback">
      <button @click="previous">
        <Icon icon="ion:play-skip-back" />
      </button>
      <button @click="changePlayback">
        <Icon :icon="playingIcon" class="play" />
      </button>
      <button @click="next">
        <Icon icon="ion:play-skip-forward" />
      </button>
    </div>
    <div class="volume">
      <button @click="toggleMuted">
        <Icon :icon="volumeIcon" />
      </button>
      <input
        type="range"
        name="volume"
        id="volume"
        min="0"
        max="100"
        v-model="volume"
        step="10"
        @input="changeVolume(volume)"
        aria-valuetext="Volume"
      />
    </div>
  </div>
</template>

<style scoped>
button {
  background: none;
  border: none;
  padding: 0.5rem;
  cursor: pointer;
}

.iconify {
  font-size: 2rem;
  color: white;
}

.play {
  font-size: 3rem;
}

.controls {
  display: grid;
  grid-template-columns: 1fr auto 1fr;
  width: 100%;
}
.playback {
  display: flex;
  align-items: center;
  justify-content: center;
  grid-column-start: 2;
}

.volume {
  margin-left: auto;
  display: flex;
  align-items: center;
}
</style>
