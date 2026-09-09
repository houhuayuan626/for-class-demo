<script setup lang="ts">
import { ref } from 'vue'
import ChildComponent from './ChildComponent.vue'

// 传给子组件的 props
const title = ref<string>('来自父组件的标题')
const count = ref<number>(0)

// 接收子组件发来的消息
const messages = ref<string[]>([])

// v-model 双向绑定的值
const sharedText = ref<string>('初始文本')

// 通过 ref 拿到子组件实例，调用其 expose 出来的方法
const childRef = ref<InstanceType<typeof ChildComponent> | null>(null)

// 2. emits：响应子组件的 increment 事件（父组件才真正修改数据）
function handleIncrement(step: number) {
  count.value += step
}

// 2. emits：接收子组件发来的消息
function handleSendMessage(message: string) {
  const time = new Date().toLocaleTimeString()
  messages.value.push(`[${time}] ${message}`)
}

// 4. defineExpose：父组件调用子组件暴露的方法
function callChildMethod() {
  childRef.value?.logFromChild('父组件调用了子组件的 logFromChild 方法')
}
function callChildClear() {
  childRef.value?.clearLogs()
}
</script>

<template>
  <div class="demo">
    <h2>Vue 父子组件通信演示</h2>

    <div class="parent">
      <h3>👨 父组件 ParentChildDemo</h3>

      <section class="block">
        <h4>父组件状态</h4>
        <p>count = <strong>{{ count }}</strong>（子组件点按钮后由父组件修改）</p>
        <p>
          双向绑定文本：<input v-model="sharedText" />
          <span class="hint">（在父组件或子组件中修改都会互相同步）</span>
        </p>
      </section>

      <section class="block">
        <h4>收到子组件的消息（emits）</h4>
        <ul class="msg-list">
          <li v-for="(msg, i) in messages" :key="i">{{ msg }}</li>
          <li v-if="!messages.length" class="hint">暂无消息</li>
        </ul>
      </section>

      <section class="block">
        <h4>调用子组件方法（defineExpose）</h4>
        <button class="btn" @click="callChildMethod">调用子组件的 logFromChild()</button>
        <button class="btn secondary" @click="callChildClear">调用子组件的 clearLogs()</button>
      </section>
    </div>

    <!--
      1. props:    :title / :count 传给子组件
      2. emits:    @increment / @send-message 监听子组件事件
      3. v-model:  双向绑定 sharedText
      4. ref:      拿到子组件实例以调用其暴露的方法
    -->
    <ChildComponent
      ref="childRef"
      :title="title"
      :count="count"
      v-model="sharedText"
      @increment="handleIncrement"
      @send-message="handleSendMessage"
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

.btn {
  padding: 4px 12px;
  border: none;
  border-radius: 4px;
  background: #35495e;
  color: #fff;
  cursor: pointer;
}

.btn.secondary {
  margin-left: 8px;
  background: #888;
}

.btn:hover {
  opacity: 0.85;
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

.msg-list {
  margin: 0;
  padding-left: 20px;
  max-height: 120px;
  overflow-y: auto;
  font-family: Consolas, monospace;
  font-size: 13px;
  line-height: 1.8;
}
</style>
