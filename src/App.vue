<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue'
import GameControls from './components/GameControls.vue'
import ScoreBoard from './components/ScoreBoard.vue'
import sevenOne from './assets/sounds/seven-1.mp3'
import sevenTwo from './assets/sounds/seven2.mp3'
import sixOne from './assets/sounds/six-1.mp3'
import sixTwo from './assets/sounds/six-2.mp3'

const SIX = [sixOne, sixTwo]
const SEVEN = [sevenOne, sevenTwo]
const STORAGE_KEY = 'six-seven-high-score'
const score = ref(0)
const highScore = ref(0)
const sequence = ref('')
const lastHit = ref('')

function playRandomSound(sounds: string[]) {
  const sound = new Audio(sounds[Math.floor(Math.random() * sounds.length)])
  void sound.play()
}

function pressNumber(number: '6' | '7') {
  playRandomSound(number === '6' ? SIX : SEVEN)

  if (number === '6') {
    sequence.value = '6'
    lastHit.value = ''
    return
  }

  if (sequence.value === '6') {
    score.value += 1
    highScore.value = Math.max(highScore.value, score.value)
    localStorage.setItem(STORAGE_KEY, String(highScore.value))
    sequence.value = ''
    window.setTimeout(() => {
      lastHit.value = ''
    }, 550)
    return
  }

  sequence.value = ''
  lastHit.value = ''
}

function handleKeydown(event: KeyboardEvent) {
  if (event.key === '6' || event.key === '7') {
    event.preventDefault()
    pressNumber(event.key)
  }
}

onMounted(() => {
  const savedHighScore = Number.parseInt(localStorage.getItem(STORAGE_KEY) ?? '0', 10)
  highScore.value = Number.isFinite(savedHighScore) ? savedHighScore : 0
  window.addEventListener('keydown', handleKeydown)
})

onUnmounted(() => window.removeEventListener('keydown', handleKeydown))
</script>

<template>
  <main class="min-h-screen bg-[#f4f1ea] font-mono text-[#173737]" aria-label="67 game">
    <GameControls @press-number="pressNumber" />
    <ScoreBoard :score="score" :high-score="highScore" />
  </main>
</template>
