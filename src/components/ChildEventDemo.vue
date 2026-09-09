<script setup lang="ts">
import { ref } from 'vue'
import EventChild from './EventChild.vue'

// 父组件收到的所有事件日志，用来直观展示"监听到了子组件事件"
const eventLogs = ref<string[]>([])

function log(text: string) {
  const time = new Date().toLocaleTimeString()
  eventLogs.value.unshift(`[${time}] ${text}`)
}

// 1. 监听 notify 事件：参数就是子组件 emit 时传过来的值
function handleNotify(message: string) {
  log(`收到 notify 事件，消息内容："${message}"`)
}

// 2. 监听 change 事件：子组件 emit 了多个参数，这里按顺序接收
function handleChange(delta: number, value: number) {
  log(`收到 change 事件，步长 ${delta > 0 ? '+' : ''}${delta}，最新值 = ${value}`)
}

// 3. 监听 submit 事件：参数是一个对象
function handleSubmit(form: { username: string; age: number }) {
  log(`收到 submit 事件，表单数据：姓名 ${form.username}，年龄 ${form.age}`)
}

// 4. 模板里用 @welcome.once 监听，只有第一次触发时才会执行
function handleWelcome(message: string) {
  log(`收到 welcome 事件（.once 只执行一次）："${message}"`)
}

function clearLogs() {
  eventLogs.value = []
}
</script>

<template>
  <div class="demo">
    <h2>监听子组件事件演示</h2>
    <p class="intro">
      子组件通过 <code>emit</code> 触发事件，父组件在模板里用
      <code>@事件名="处理函数"</code> 监听，事件参数会直接传给处理函数。
    </p>

    <div class="parent">
      <h3>👨 父组件 ChildEventDemo（事件的监听方）</h3>

      <section class="block">
        <h4>事件监听日志 <button class="btn secondary" @click="clearLogs">清空</button></h4>
        <ul class="log-list">
          <li v-for="(item, i) in eventLogs" :key="i">{{ item }}</li>
          <li v-if="!eventLogs.length" class="hint">暂无日志，去下面操作子组件试试</li>
        </ul>
      </section>
    </div>

    <!--
      @notify="handleNotify"        → 监听子组件的 notify 事件
      @change="handleChange"        → 监听 change 事件（接收多个参数）
      @submit="handleSubmit"        → 监听 submit 事件（接收对象参数）
      @welcome.once="handleWelcome" → .once 修饰符：只监听一次
    -->
    <EventChild
      @notify="handleNotify"
      @change="handleChange"
      @submit="handleSubmit"
      @welcome.once="handleWelcome"
    />
  </div>
</template>

<style scoped>
.demo {
  max-width: 640px;
  margin: 0 auto;
  padding: 16px;
  font-family: 'Microsoft YaHei', sans-serif;
}

.intro {
  text-align: center;
  color: #555;
  font-size: 14px;
}

.intro code {
  background: #eee;
  padding: 1px 6px;
  border-radius: 4px;
  color: #d63384;
}

.parent {
  border: 2px solid #35495e;
  border-radius: 8px;
  padding: 12px 16px;
  margin-bottom: 16px;
  text-align: left;
  background: #f8f9fb;
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
  color: #35495e;
}

.btn.secondary {
  margin-left: 8px;
  padding: 2px 10px;
  border: none;
  border-radius: 4px;
  background: #888;
  color: #fff;
  cursor: pointer;
  font-size: 12px;
}

.btn:hover {
  opacity: 0.85;
}

.log-list {
  margin: 0;
  padding-left: 20px;
  max-height: 160px;
  overflow-y: auto;
  font-family: Consolas, monospace;
  font-size: 13px;
  line-height: 1.8;
}

.hint {
  color: #888;
  font-size: 13px;
}
</style>
