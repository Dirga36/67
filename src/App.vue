<script setup lang="ts"></script>

<template>
  <!-- Main container taking full viewport height and width -->
  <div class="flex w-full h-screen">
    
    <!-- 1st Area: Green -->
    <button class="flex flex-1 flex-col items-center justify-center bg-green-500 p-6 text-center active:scale-95 transition-all duration-200 ease-in-out">
      <h1 class="text-9xl font-bold text-white mb-4">6</h1>
    </button>

    <!-- 2nd Area: Red -->
    <button class="flex flex-1 flex-col items-center justify-center bg-red-500 p-6 text-center active:scale-95 transition-all duration-200 ease-in-out">
      <h1 class="text-9xl font-bold text-white mb-4">7</h1>
    </button>

  </div>
</template>
