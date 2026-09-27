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
    <button class="border-none bg-blue-500 shadow-md transform transition-all duration-150 ease-in-out hover:bg-indigo-700 active:scale-95 active:shadow-sm" aria-label="Press 6" @click="emit('pressNumber', '6')">
      <span class="text-white text-9xl">6</span>
    </button>
    <button class="border-none bg-green-500 shadow-md transform transition-all duration-150 ease-in-out hover:bg-indigo-700 active:scale-95 active:shadow-sm" aria-label="Press 7" @click="emit('pressNumber', '7')">
      <span class="text-white text-9xl">7</span>
    </button>
  </section>
</template>
