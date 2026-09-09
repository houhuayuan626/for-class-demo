<script setup lang="ts">
import { ref } from 'vue'

// ========== 1. props：父组件 → 子组件（单向数据流） ==========
const props = defineProps<{
  title: string
  count: number
}>()

// ========== 2. emits：子组件 → 父组件（自定义事件） ==========
const emit = defineEmits<{
  // 子组件点击按钮，通知父组件让 count +1
  (e: 'increment', step: number): void
  // 子组件向父组件发送一条文本消息
  (e: 'sendMessage', message: string): void
}>()

function onIncrementClick() {
  // 子组件不直接修改 props，而是通过事件通知父组件去改
  emit('increment', 1)
}

const message = ref<string>('')
function onSendClick() {
  if (!message.value.trim()) return
  emit('sendMessage', message.value)
  message.value = ''
}

// ========== 3. v-model：父子双向绑定 ==========
// defineModel() 是 Vue 3.4+ 提供的语法糖，等价于
// props: modelValue + emits: update:modelValue
const inputValue = defineModel<string>({ default: '' })

// ========== 4. defineExpose：暴露方法给父组件（父 → 子 的另一种方式） ==========
const childLogs = ref<string[]>([])
function logFromChild(text: string) {
  const time = new Date().toLocaleTimeString()
  childLogs.value.push(`[${time}] ${text}`)
}
function clearLogs() {
  childLogs.value = []
}

defineExpose({
  logFromChild,
  clearLogs,
})
</script>

<template>
  <div class="child">
    <h3>👦 子组件 ChildComponent</h3>

    <!-- props 演示 -->
    <section class="block">
      <h4>1. props（父 → 子）</h4>
      <p>父组件传来的标题：<strong>{{ props.title }}</strong></p>
      <p>
        父组件传来的计数：<strong>{{ props.count }}</strong>
        <button class="btn" @click="onIncrementClick">通知父组件 +1</button>
      </p>
    </section>

    <!-- emits 演示 -->
    <section class="block">
      <h4>2. emits（子 → 父）</h4>
      <input v-model="message" placeholder="输入要发送给父组件的消息" @keyup.enter="onSendClick" />
      <button class="btn" @click="onSendClick">发送消息给父组件</button>
    </section>

    <!-- v-model 演示 -->
    <section class="block">
      <h4>3. v-model（双向绑定）</h4>
      <input v-model="inputValue" placeholder="这里修改会同步到父组件" />
      <p class="hint">当前值：{{ inputValue || '（空）' }}</p>
    </section>

    <!-- defineExpose 演示 -->
    <section class="block">
      <h4>4. defineExpose（父调用子方法）</h4>
      <p class="hint">父组件可以通过 ref 调用子组件暴露的方法，往这里写日志：</p>
      <ul class="child-log">
        <li v-for="(log, i) in childLogs" :key="i">{{ log }}</li>
        <li v-if="!childLogs.length" class="hint">暂无日志，点击父组件中的按钮试试</li>
      </ul>
    </section>
  </div>
</template>

<style scoped>
.child {
  border: 2px dashed #42b883;
  border-radius: 8px;
  padding: 12px 16px;
  background: #f0fdf7;
  text-align: left;
}

.block {
  margin: 12px 0;
  padding: 8px 12px;
  background: #fff;
  border-radius: 6px;
  border: 1px solid #e0e0e0;
}

.block h4 {
  margin: 0 0 8px;
  color: #42b883;
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

.btn:hover {
  background: #369870;
}

input {
  padding: 6px 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  width: 220px;
}

.hint {
  color: #888;
  font-size: 13px;
}

.child-log {
  margin: 0;
  padding-left: 20px;
  max-height: 120px;
  overflow-y: auto;
  font-family: Consolas, monospace;
  font-size: 13px;
  line-height: 1.8;
}
</style>
