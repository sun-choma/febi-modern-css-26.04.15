<template>
  <div class="shape-demo">
    <div class="shape" :style="{ cornerShape: shape, borderRadius: '2.5rem' }">
      <span class="label">{{ shape }}</span>
    </div>
  </div>
</template>

<script setup>
defineProps({
  shape: {
    type: String,
    default: 'round'
  }
})
</script>

<style scoped>
.shape-demo {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1rem;
}

.shape {
  width: 120px;
  height: 120px;
  background: #bd93f9;
  display: flex;
  align-items: center;
  justify-content: center;
}

.label {
  font-size: 0.75rem;
  font-weight: bold;
  color: #282a36;
}
</style>