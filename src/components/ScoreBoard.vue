<script setup lang="ts">
defineProps<{
  score: number
  highScore: number
}>()
</script>

<template>
  <section
    class="flex min-h-[120px] items-center justify-center gap-7 px-5 sm:min-h-[128px] sm:gap-10"
    aria-live="polite"
  >
    <div class="flex flex-col">
      <span class="text-[11px] font-bold uppercase tracking-[.14em] text-[#7ca7a2]">Score</span
      ><strong class="text-[clamp(3.5rem,7vw,5rem)] leading-[.8] text-[#003e4b]"
        >{{ score .toString() .padStart(2, '0') }}</strong
      >
    </div>
    <div class="h-14 w-px bg-[#c8cbc4]"></div>
    <div class="flex flex-col">
      <span class="text-[11px] font-bold uppercase tracking-[.14em] text-[#8c9a9b]">High score</span
      ><strong class="text-[clamp(3.5rem,7vw,5rem)] leading-[.8] text-[#759096]"
        >{{ highScore .toString() .padStart(2, '0') }}</strong
      >
    </div>
  </section>
</template>
