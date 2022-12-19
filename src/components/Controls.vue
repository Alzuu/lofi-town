<script setup>
import { ref, computed, onMounted } from 'vue'

const props = defineProps(['player'])

const volume = ref(100)
const isPlaying = ref(false)
const isMuted = ref(false)

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
}

function next() {
  console.log('next video')
  props.player.nextVideo()
}

function changeVolume(volume) {
  console.log('change volume to ' + volume)
  props.player.setVolume(volume)
}

function toggleMuted() {
  if (isMuted.value) {
    console.log('unmuted')
    props.player.unMute()
  } else {
    console.log('muted')
    props.player.mute()
  }
  isMuted.value = !isMuted.value
}

const playingText = computed(() => {
  return isPlaying.value ? 'Pause' : 'Play'
})

const mutedText = computed(() => {
  return isMuted.value ? 'Unmute' : 'Mute'
})
</script>

<template>
  <div>
    <button @click="changePlayback">{{ playingText }}</button>
    <button @click="previous">Previous</button>
    <button @click="next">Next</button>
    <button @click="toggleMuted">{{ mutedText }}</button>
    <input
      type="range"
      name="volume"
      id="volume"
      min="0"
      max="100"
      v-model="volume"
      step="10"
      @input="changeVolume(volume)"
    />
  </div>
</template>

<style scoped></style>
