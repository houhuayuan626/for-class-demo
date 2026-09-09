<script setup lang="ts">
import {
  ref,
  onBeforeMount,
  onMounted,
  onBeforeUpdate,
  onUpdated,
  onBeforeUnmount,
  onUnmounted,
} from 'vue'

const count = ref<number>(0)
const logs = ref<string[]>([])

function addLog(message: string) {
  const time = new Date().toLocaleTimeString()
  logs.value.push(`[${time}] ${message}`)
  console.log(message)
}

// setup 阶段本身相当于 beforeCreate / created
addLog('setup: 组件初始化（相当于 beforeCreate / created）')

onBeforeMount(() => addLog('onBeforeMount: 组件挂载前'))
onMounted(() => addLog('onMounted: 组件挂载完成'))
onBeforeUpdate(() => addLog(`onBeforeUpdate: 数据更新前（count 即将变为 ${count.value}）`))
onUpdated(() => addLog(`onUpdated: 组件更新完成（当前 count = ${count.value}）`))
onBeforeUnmount(() => addLog('onBeforeUnmount: 组件卸载前'))
onUnmounted(() => addLog('onUnmounted: 组件已卸载'))

function increment() {
  count.value++
}
</script>

<template>
  <div class="demo">
    <h2>Vue 生命周期函数演示</h2>

    <div class="counter">
      <p>
        计数器：<strong>{{ count }}</strong>
      </p>
      <button @click="increment">点我 +1</button>
    </div>

    <div class="log-panel">
      <h3>生命周期日志</h3>
      <ul>
        <li v-for="(log, index) in logs" :key="index">{{ log }}</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.demo {
  max-width: 520px;
  margin: 0 auto;
  padding: 16px;
  font-family: 'Microsoft YaHei', sans-serif;
}

.counter {
  margin: 16px 0;
}

.counter button {
  padding: 8px 20px;
  font-size: 16px;
  border: none;
  border-radius: 6px;
  background-color: #42b883;
  color: #fff;
  cursor: pointer;
}

.counter button:hover {
  background-color: #369870;
}

.log-panel {
  text-align: left;
  background: #f6f8fa;
  border: 1px solid #ddd;
  border-radius: 6px;
  padding: 12px;
}

.log-panel ul {
  margin: 0;
  padding-left: 20px;
  max-height: 240px;
  overflow-y: auto;
}

.log-panel li {
  font-family: Consolas, monospace;
  font-size: 13px;
  line-height: 1.8;
}
</style>
