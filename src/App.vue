<script setup lang="ts">
import { computed, onMounted, onUnmounted, ref } from 'vue'

const STORAGE_KEY = 'six-seven-high-score'
const score = ref(0)
const highScore = ref(0)
const sequence = ref('')
const lastHit = ref(false)

const progressLabel = computed(() => (sequence.value === '6' ? '6?' : '...'))

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
  <main class="min-h-screen bg-[#f4f1ea] font-mono text-[#173737]" aria-label="67 game">
    <section
      class="grid h-[calc(100vh-120px)] min-h-[470px] grid-cols-2 border-b-[8px] border-black sm:h-[calc(100vh-128px)]"
      aria-label="Game controls"
    >
      <button
        class="border-none"
        aria-label="Press 6"
        @click="pressNumber('6')"
      >
      </button>
      <button
        class="border-none"
        aria-label="Press 7"
        @click="pressNumber('7')"
      >
      </button>
    </section>

    <section
      class="flex min-h-[120px] items-center justify-center gap-7 px-5 sm:min-h-[128px] sm:gap-10"
      aria-live="polite"
    >
      <div class="flex flex-col">
        <span class="text-[11px] font-bold uppercase tracking-[.14em] text-[#7ca7a2]">Score</span
        ><strong
          class="text-[clamp(3.5rem,7vw,5rem)] leading-[.8]  text-[#003e4b]"
          >{{ score.toString().padStart(2, '0') }}</strong
        >
      </div>
      <div class="h-14 w-px bg-[#c8cbc4]"></div>
      <div class="flex flex-col">
        <span class="text-[11px] font-bold uppercase tracking-[.14em] text-[#8c9a9b]"
          >High score</span
        ><strong
          class="text-[clamp(2.8rem,5vw,4.5rem)] leading-[.8]  text-[#759096]"
          >{{ highScore.toString().padStart(2, '0') }}</strong
        >
      </div>
    </section>
    <p aria-live="polite">
      {{ lastHit ? 'Nice one. Keep going!' : progressLabel }}
    </p>
  </main>
</template>
