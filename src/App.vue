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
  <main class="min-h-screen bg-[#f4f1ea] px-5 py-6 text-[#173737] sm:px-10 lg:px-20 lg:py-8">
    <div class="mx-auto grid min-h-[calc(100vh-5rem)] w-full max-w-6xl items-center gap-10 lg:grid-cols-[.88fr_1.12fr] lg:gap-24" aria-label="67 game">
      <section class="pb-4">
        <div class="mb-6 flex items-center gap-2 text-[11px] font-bold uppercase tracking-[.13em] text-[#75817a]"><span class="size-2 rounded-full bg-[#d95c3c] ring-4 ring-[#f0d4c9]"></span> Quick reflex game</div>
        <h1 class="text-[clamp(3.6rem,7vw,6.25rem)] font-extrabold leading-[.92] tracking-[-.075em]">Can you<br /><span class="text-[#d95c3c]">make 67?</span></h1>
        <p class="my-7 text-[17px] leading-relaxed text-[#71807b] lg:mb-10">Press the numbers in order.<br />Every <strong class="font-extrabold text-[#d95c3c]">6 → 7</strong> earns a point.</p>

        <div class="flex items-center gap-6" aria-live="polite">
          <div class="flex flex-col gap-0.5"><span class="text-[10px] font-bold uppercase tracking-[.13em] text-[#89948c]">Score</span><strong class="text-5xl leading-none tracking-[-.06em]">{{ score.toString().padStart(2, '0') }}</strong></div>
          <div class="h-10 w-px bg-[#d4d5cc]"></div>
          <div class="flex flex-col gap-0.5"><span class="text-[10px] font-bold uppercase tracking-[.13em] text-[#89948c]">High score</span><strong class="text-3xl leading-none tracking-[-.05em] text-[#75817a]">{{ highScore.toString().padStart(2, '0') }}</strong></div>
        </div>
        <div class="mt-7 flex items-center gap-2 text-[9px] font-bold uppercase tracking-[.13em] text-[#9aa29b] lg:mt-12"><span class="grid size-[22px] place-items-center rounded-md border border-[#c6cbc2] text-sm tracking-normal">⌨</span> Use your keyboard or tap the buttons</div>
      </section>

      <section aria-label="Game controls">
        <div class="mb-4 flex items-center justify-between text-[11px] font-bold uppercase tracking-[.13em] text-[#89948c]"><span class="text-[#d95c3c]">Round {{ score + 1 }}</span><span class="text-xs normal-case tracking-normal">{{ progressLabel }}</span></div>
        <div class="grid grid-cols-2 gap-3.5">
          <button class="relative flex aspect-[.95] flex-col items-center justify-center overflow-hidden rounded-[28px] bg-[#3c7973] text-white shadow-[0_16px_30px_rgba(48,58,49,.13)] transition hover:-translate-y-1 hover:shadow-[0_21px_32px_rgba(48,58,49,.2)] active:translate-y-0.5 active:scale-[.98] focus-visible:outline-4 focus-visible:outline-[#173737] focus-visible:outline-offset-4" :class="sequence === '6' ? 'ring-4 ring-[#f4f1ea] ring-offset-2 ring-offset-[#3c7973]' : ''" aria-label="Press 6" @click="pressNumber('6')"><span class="absolute inset-2.5 rounded-[20px] border border-white/30"></span><span class="relative text-[clamp(5.75rem,12vw,10.4rem)] font-extrabold leading-[.8] tracking-[-.1em]">6</span><span class="relative mt-5 text-[10px] font-bold uppercase tracking-[.13em] opacity-70">first</span></button>
          <button class="relative flex aspect-[.95] flex-col items-center justify-center overflow-hidden rounded-[28px] bg-[#d95c3c] text-white shadow-[0_16px_30px_rgba(48,58,49,.13)] transition hover:-translate-y-1 hover:shadow-[0_21px_32px_rgba(48,58,49,.2)] active:translate-y-0.5 active:scale-[.98] focus-visible:outline-4 focus-visible:outline-[#173737] focus-visible:outline-offset-4" aria-label="Press 7" @click="pressNumber('7')"><span class="absolute inset-2.5 rounded-[20px] border border-white/30"></span><span class="relative text-[clamp(5.75rem,12vw,10.4rem)] font-extrabold leading-[.8] tracking-[-.1em]">7</span><span class="relative mt-5 text-[10px] font-bold uppercase tracking-[.13em] opacity-70">second</span></button>
        </div>
        <div class="flex min-h-[50px] items-center justify-center gap-2 text-[13px] font-semibold" :class="lastHit ? 'text-[#3c7973]' : 'text-[#89948c]'" aria-live="polite"><span class="text-xl">{{ lastHit ? '✓' : '→' }}</span><span>{{ lastHit ? 'Nice one. Keep going!' : 'The order matters.' }}</span></div>
      </section>
    </div>
    <footer class="mx-auto mt-5 w-full max-w-6xl text-[9px] font-bold uppercase tracking-[.13em] text-[#a2a79f]"><span class="text-[#d95c3c]">67</span> — simple numbers, serious focus.</footer>
  </main>
</template>
