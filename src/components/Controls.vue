<script setup>
import { ref, computed, onMounted, inject } from 'vue'

import { Icon } from '@iconify/vue'

const props = defineProps(['player', 'title'])

const volume = ref(100)
const isPlaying = ref(false)

const { changeCurrentGif } = inject('gif')

function changePlayback() {
  if (isPlaying.value) {
    props.player.pauseVideo()
  } else {
    props.player.playVideo()
  }
  isPlaying.value = !isPlaying.value
}

function previous() {
  props.player.previousVideo()
  isPlaying.value = true
  changeCurrentGif()
}

function next() {
  props.player.nextVideo()
  isPlaying.value = true
  changeCurrentGif()
}

function changeVolume(currentVolume) {
  if (currentVolume <= 100 && currentVolume >= 0) {
    props.player.setVolume(currentVolume)
    volume.value = currentVolume
  }
}

function toggleMuted() {
  const playerVolume = props.player.getVolume()
  if (volume.value == 0) {
    props.player.unMute()
    volume.value = playerVolume > 0 ? playerVolume : 10
  } else {
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

onMounted(() => {
  window.addEventListener('keyup', (event) => {
    if (event.code === 'Space') {
      changePlayback()
    }
    if (event.code === 'ArrowLeft') {
      previous()
    }
    if (event.code === 'ArrowRight') {
      next()
    }
    if (event.code === 'ArrowUp') {
      changeVolume(volume.value + 10)
    }
    if (event.code === 'ArrowDown') {
      changeVolume(volume.value - 10)
    }
    if (event.code === 'KeyM') {
      toggleMuted()
    }
  })

  // Removes focus after any element is clicked
  window.addEventListener('mouseup', (event) => {
    event.target.blur()
  })
})
</script>

<template>
  <div class="controls-wrapper">
    <p>{{ props.title }}</p>
    <div class="controls">
      <button @click="previous" aria-label="Skip backwards">
        <Icon icon="ion:play-skip-back" />
      </button>
      <button
        @click="changePlayback"
        :aria-label="isPlaying ? 'Pause' : 'Play'"
      >
        <Icon :icon="playingIcon" class="play" />
      </button>
      <button @click="next" aria-label="Skip forwards">
        <Icon icon="ion:play-skip-forward" />
      </button>
      <div class="volume">
        <button
          @click="toggleMuted"
          :aria-label="volume > 0 ? 'Mute' : 'Unmute'"
        >
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
          aria-valuemin="0"
          aria-valuemax="100"
          :aria-valuenow="volume"
          aria-label="Volume slider"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
button {
  background: none;
  border: none;
  cursor: pointer;
}

.iconify {
  font-size: 1.5rem;
  color: #9f9f9f;
}

.iconify:hover {
  color: #fff;
  transition: 200ms ease-in-out;
}

.iconify:hover {
  color: #fff;
}

.play {
  font-size: 3rem;
  color: #fff;
}

.play:active {
  transform: scale(0.9);
}

.controls-wrapper {
  display: grid;
  grid-template-rows: 1fr 1fr;
  width: 100%;
  background-color: #252525;
  border: 2px solid #252525;
  border-radius: 1rem;
  padding: 1rem;
}

.controls {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.volume {
  display: flex;
  align-items: center;
  gap: 0.2rem;
}

p {
  margin: 0.7rem;
  max-width: 80vw;
  font-weight: 500;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: clip;
}

body {
  height: 100vh;
  margin: 0;
  display: flex;
}

input[type='range'] {
  -webkit-appearance: none;
  overflow: hidden;
  height: 1.2rem;
  width: 100%;
  cursor: pointer;
  border-radius: 0; /* iOS */
  border: 2px solid #fff;
}

::-webkit-slider-runnable-track {
  background: #252525;
}

::-webkit-slider-thumb {
  -webkit-appearance: none;
  width: 0px;
  height: 1.2rem;
  box-shadow: -100px 0 0 100px white;
}

::-moz-range-track {
  height: 1.2rem;
  background: #252525;
}

::-moz-range-thumb {
  height: 1.2rem;
  width: 0px;
  border: none;
  box-shadow: -100px 0 0 100px white;
  box-sizing: border-box;
}

@media (min-width: 640px) {
  .controls-wrapper {
    width: 60%;
  }
}

@media (min-width: 768px) {
  input[type='range'] {
    width: 10rem;
  }
}

@media (min-width: 1024px) {
  .controls-wrapper {
    width: 45%;
  }
}
</style>
