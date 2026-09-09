<script setup lang="ts">
import { ref } from 'vue'

// SiblingA 是"发送方"：通过 emit 把消息交给父组件
const emit = defineEmits<{
  // 发送消息给兄弟组件（由父组件中转）
  (e: 'send', message: string): void
  // 请求清空兄弟组件的消息列表（由父组件中转）
  (e: 'clear'): void
}>()

const message = ref<string>('')

function onSend() {
  if (!message.value.trim()) return
  // 兄弟组件之间不能直接通信，先把消息 emit 给父组件
  emit('send', message.value)
  message.value = ''
}
</script>

<template>
  <div class="sibling sibling-a">
    <h4>🧑 兄弟组件 A（发送方）</h4>
    <input v-model="message" placeholder="输入要发给 B 的消息" @keyup.enter="onSend" />
    <button class="btn" @click="onSend">发送给 B</button>
    <button class="btn secondary" @click="emit('clear')">清空 B 的消息</button>
    <p class="hint">A 拿不到 B 的实例，只能 emit 事件给共同的父组件</p>
  </div>
</template>

<style scoped>
.sibling {
  flex: 1;
  border: 2px dashed #42b883;
  border-radius: 8px;
  padding: 12px 16px;
  background: #f0fdf7;
  text-align: left;
}

.sibling h4 {
  margin: 0 0 8px;
  color: #42b883;
}

input {
  padding: 6px 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  width: 180px;
}

.btn {
  margin-left: 8px;
  padding: 4px 12px;
  border: none;
  border-radius: 4px;
  background: #42b883;
  color: #fff;
  cursor: pointer;
}

.btn.secondary {
  background: #888;
}

.btn:hover {
  opacity: 0.85;
}

.hint {
  color: #888;
  font-size: 13px;
  margin-bottom: 0;
}
</style>
