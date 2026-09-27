<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue'
import GameControls from './components/GameControls.vue'
import ScoreBoard from './components/ScoreBoard.vue'

const STORAGE_KEY = 'six-seven-high-score'
const score = ref(0)
const highScore = ref(0)
const sequence = ref('')
const lastHit = ref('')

function pressNumber(number: '6' | '7') {
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
