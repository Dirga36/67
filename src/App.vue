<script setup lang="ts">
import { computed, onMounted, onUnmounted, ref } from 'vue'

const STORAGE_KEY = 'sixty-seven-high-score'
const score = ref(0)
const highScore = ref(0)
const sequence = ref('')
const lastHit = ref(false)

const progressLabel = computed(() => (sequence.value === '6' ? 'Now tap 7' : 'Start with 6'))

function pressNumber(number: '6' | '7') {
  if (number === '6') {
    sequence.value = '6'
    lastHit.value = false
    return
  }

  if (sequence.value === '6') {
    score.value += 1
    highScore.value = Math.max(highScore.value, score.value)
    localStorage.setItem(STORAGE_KEY, String(highScore.value))
    sequence.value = ''
    lastHit.value = true
    window.setTimeout(() => {
      lastHit.value = false
    }, 550)
    return
  }

  sequence.value = ''
  lastHit.value = false
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
  <main class="game-shell">
    <div class="game-grid" aria-label="67 game">
      <section class="game-copy">
        <div class="eyebrow"><span class="live-dot"></span> Quick reflex game</div>
        <h1>Can you<br /><em>make 67?</em></h1>
        <p class="intro">Press the numbers in order.<br />Every <strong>6 → 7</strong> earns a point.</p>

        <div class="score-row" aria-live="polite">
          <div class="score-card">
            <span class="label">Score</span>
            <strong>{{ score.toString().padStart(2, '0') }}</strong>
          </div>
          <div class="divider"></div>
          <div class="high-card">
            <span class="label">High score</span>
            <strong>{{ highScore.toString().padStart(2, '0') }}</strong>
          </div>
        </div>

        <div class="hint"><span class="key-hint">⌨</span> Use your keyboard or tap the buttons</div>
      </section>

      <section class="play-panel" aria-label="Game controls">
        <div class="panel-topline">
          <span class="round-tag">ROUND {{ score + 1 }}</span>
          <span class="progress-text">{{ progressLabel }}</span>
        </div>

        <div class="number-pad">
          <button class="number-button six" :class="{ selected: sequence === '6' }" aria-label="Press 6" @click="pressNumber('6')">
            <span class="button-number">6</span>
            <span class="button-caption">first</span>
          </button>
          <button class="number-button seven" aria-label="Press 7" @click="pressNumber('7')">
            <span class="button-number">7</span>
            <span class="button-caption">second</span>
          </button>
        </div>

        <div class="feedback" :class="{ success: lastHit }" aria-live="polite">
          <span class="feedback-mark">{{ lastHit ? '✓' : '→' }}</span>
          <span>{{ lastHit ? 'Nice one. Keep going!' : 'The order matters.' }}</span>
        </div>
      </section>
    </div>
    <footer><span>67</span> — simple numbers, serious focus.</footer>
  </main>
</template>
