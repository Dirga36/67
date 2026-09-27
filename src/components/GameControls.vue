<script setup lang="ts">
const emit = defineEmits<{
  pressNumber: [number: '6' | '7']
}>()
</script>

<template>
  <section
    class="grid h-[calc(100vh-120px)] min-h-[470px] grid-cols-2 border-b-[8px] border-black sm:h-[calc(100vh-128px)]"
    aria-label="Game controls"
  >
    <button class="border-none" aria-label="Press 6" @click="emit('pressNumber', '6')"></button>
    <button class="border-none" aria-label="Press 7" @click="emit('pressNumber', '7')"></button>
  </section>
</template>
