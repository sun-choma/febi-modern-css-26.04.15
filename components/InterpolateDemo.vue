<template>
  <div class="demo">
    <button @click="open = !open" class="toggle">
      {{ open ? '閉じる' : 'もっと見る' }}
    </button>
    <div class="content" :class="{ open }">
      <p>CSSが <code>auto</code>、<code>max-content</code>、<code>fit-content</code> などのintrinsicサイズキーワードへのアニメーションに対応。JS
        による高さ計算は不要。</p>
      <p>以前は <code>max-height</code> トランジションのハックや、JS で <code>scrollHeight</code> を測る方法に頼っていた。今は
        <code>height: auto</code> と書くだけ。</p>
    </div>
  </div>
</template>

<script setup>
import {ref} from 'vue'

const open = ref(false)
</script>

<style scoped>
.demo {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  max-width: 400px;
  margin: 0 auto;
}

.toggle {
  padding: 0.5rem 1rem;
  background: #bd93f9;
  color: #282a36;
  border: none;
  border-radius: 0.5rem;
  cursor: pointer;
  font-weight: bold;
  align-self: flex-start;
}

.content {
  interpolate-size: allow-keywords;
  height: 0;
  overflow: hidden;
  transition: height 0.5s ease;
  color: #f8f8f2;
}

.content.open {
  height: auto;
}

.content p {
  padding: 0.75rem;
  margin: 0;
  background: #44475a;
  border-radius: 0.5rem;
}

.content p + p {
  margin-top: 0.5rem;
}
</style>