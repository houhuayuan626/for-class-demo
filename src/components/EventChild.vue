<script setup lang="ts">
import { ref } from 'vue'

// ========== defineEmits：声明子组件可以触发的自定义事件 ==========
// 声明后父组件就可以用 @事件名 来监听，TS 还能检查参数类型
const emit = defineEmits<{
  // 普通通知事件，携带一条文本
  (e: 'notify', message: string): void
  // 计数变化事件，携带 步长 和 最新值 两个参数
  (e: 'change', delta: number, value: number): void
  // 表单提交事件，携带一个对象作为参数
  (e: 'submit', form: { username: string; age: number }): void
  // 只触发一次的欢迎事件（配合父组件的 .once 修饰符）
  (e: 'welcome', message: string): void
}>()

// 子组件内部维护自己的计数，变化时通过事件把结果通知给父组件
const localCount = ref<number>(0)
function onAdd() {
  localCount.value += 1
  // emit 可以携带多个参数，父组件的回调会依次收到
  emit('change', 1, localCount.value)
}
function onMinus() {
  localCount.value -= 1
  emit('change', -1, localCount.value)
}

// 发送一条普通消息
const text = ref<string>('')
function onNotify() {
  if (!text.value.trim()) return
  emit('notify', text.value)
  text.value = ''
}

// 提交表单：事件参数可以是一个对象
const username = ref<string>('小明')
const age = ref<number>(10)
function onSubmit() {
  emit('submit', { username: username.value, age: age.value })
}

// 触发 welcome 事件（父组件用 .once 监听，只有第一次有效）
function onWelcome() {
  emit('welcome', '你好，我是子组件！')
}
</script>

<template>
  <div class="event-child">
    <h3>👦 子组件 EventChild（事件的发送方）</h3>

    <section class="block">
      <h4>触发 change 事件（携带多个参数）</h4>
      <p>
        子组件内部计数：<strong>{{ localCount }}</strong>
        <button class="btn" @click="onMinus">-1</button>
        <button class="btn" @click="onAdd">+1</button>
      </p>
      <p class="hint">点击后会 emit('change', 步长, 最新值)</p>
    </section>

    <section class="block">
      <h4>触发 notify 事件（携带文本）</h4>
      <input v-model="text" placeholder="输入消息" @keyup.enter="onNotify" />
      <button class="btn" @click="onNotify">发送</button>
    </section>

    <section class="block">
      <h4>触发 submit 事件（携带对象）</h4>
      <p>
        姓名：<input v-model="username" class="short" />
        年龄：<input v-model.number="age" type="number" class="short" />
        <button class="btn" @click="onSubmit">提交</button>
      </p>
    </section>

    <section class="block">
      <h4>触发 welcome 事件（父组件用 .once 监听）</h4>
      <button class="btn" @click="onWelcome">打招呼（只有第一次会被父组件收到）</button>
    </section>
  </div>
</template>

<style scoped>
.event-child {
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

input.short {
  width: 100px;
  margin-left: 4px;
}

.hint {
  color: #888;
  font-size: 13px;
}
</style>
