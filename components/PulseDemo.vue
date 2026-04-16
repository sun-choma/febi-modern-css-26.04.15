<template>
  <div class="flex flex-col items-center gap-3 p-4">
    <div class="flex gap-3">
      <div
          v-for="i in 4"
          :key="i"
          class="w-14 h-14 rounded-lg cursor-pointer"
          :class="[variant === 'classic' ? 'bg-pink-400' : 'bg-purple-400', { 'classic-pulse': variant === 'classic' && pulsing.has(i), 'pulsing': variant === 'property' && pulsing.has(i) }]"
          @click="toggle(i)"
      />
    </div>
  </div>
</template>

<script setup>
import {ref} from 'vue'

const props = defineProps({
  variant: {
    type: String,
    default: 'property' // 'classic' | 'property'
  }
})

const pulsing = ref(new Set([1, 2, 3, 4]))

function toggle(i) {
  if (pulsing.value.has(i)) {
    pulsing.value.delete(i)
  } else {
    pulsing.value.add(i)
  }
  pulsing.value = new Set(pulsing.value)
}
</script>

<style>
@property --opacity {
  syntax: '<number>';
  inherits: true;
  initial-value: 1;
}

@keyframes pulse-global {
  0% {
    --opacity: 1;
  }
  50% {
    --opacity: 0.2;
  }
  100% {
    --opacity: 1;
  }
}

@keyframes pulse-classic {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0.2;
  }
  100% {
    opacity: 1;
  }
}

:root {
  animation: pulse-global 1.5s ease infinite;
}

.classic-pulse {
  animation: pulse-classic 1.5s ease infinite;
}

.pulsing {
  opacity: var(--opacity);
}
</style>