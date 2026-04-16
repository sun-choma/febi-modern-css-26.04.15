<template>
  <div class="flex justify-center items-center h-32">
    <button @click="open" class="trigger">
      Dialogを開く
    </button>

    <dialog ref="dialogRef" @click.self="close" class="demo-dialog">
      <div class="dialog-content">
        <h3>ネイティブダイアログ</h3>
        <p><code>@starting-style</code> と <code>transition-behavior</code> でアニメーション。JavaScriptは一切不要 —
          純粋なCSSだけ。</p>
        <button @click="close" class="close-btn">閉じる</button>
      </div>
    </dialog>
  </div>
</template>

<script setup>
import {ref} from 'vue'

const dialogRef = ref(null)

function open() {
  dialogRef.value.showModal()
}

function close() {
  dialogRef.value.close()
}
</script>

<style scoped>
.trigger {
  padding: 0.5rem 1.5rem;
  background: #bd93f9;
  color: #282a36;
  border: none;
  border-radius: 0.5rem;
  cursor: pointer;
  font-weight: bold;
}

.demo-dialog {
  border: none;
  border-radius: 0.75rem;
  padding: 0;
  background: #44475a;
  color: #f8f8f2;
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.5);

  opacity: 1;
  transform: translateY(0) scale(1);
  transition: opacity 0.3s ease,
  transform 0.3s ease,
  display 0.3s allow-discrete,
  overlay 0.3s allow-discrete;
}

.demo-dialog:not([open]) {
  opacity: 0;
  transform: translateY(1rem) scale(0.95);
}

@starting-style {
  .demo-dialog[open] {
    opacity: 0;
    transform: translateY(1rem) scale(0.95);
  }
}

.demo-dialog::backdrop {
  background: rgba(0, 0, 0, 0);
  transition: background 0.3s ease,
  display 0.3s allow-discrete,
  overlay 0.3s allow-discrete;
}

.demo-dialog[open]::backdrop {
  background: rgba(0, 0, 0, 0.5);
}

@starting-style {
  .demo-dialog[open]::backdrop {
    background: rgba(0, 0, 0, 0);
  }
}

.dialog-content {
  padding: 2rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.dialog-content h3 {
  margin: 0;
  color: #bd93f9;
}

.dialog-content p {
  margin: 0;
  opacity: 0.8;
}

.close-btn {
  align-self: flex-end;
  padding: 0.4rem 1rem;
  background: #6272a4;
  color: #f8f8f2;
  border: none;
  border-radius: 0.5rem;
  cursor: pointer;
}
</style>