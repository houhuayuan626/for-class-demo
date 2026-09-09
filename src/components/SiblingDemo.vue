<script setup lang="ts">
import { ref } from 'vue'
import SiblingA from './SiblingA.vue'
import SiblingB from './SiblingB.vue'

// ============================================================
// 兄弟组件通信的核心思路：状态提升（Lifting State Up）
// A 和 B 是兄弟，互相拿不到对方，所以把共享数据放到共同父组件里：
//   A 发消息  →  emit 给父组件（子 → 父）
//   父组件存起来 → 通过 props 传给 B（父 → 子）
// ============================================================

// 父组件保管的共享数据
const messages = ref<string[]>([])
const theme = ref<string>('#e67e22')

// 中转站 1：接收 A 发来的消息，存进自己的状态
// 由于 B 通过 props 绑定了 messages，数据变化会自动流向 B
function handleSend(message: string) {
  const time = new Date().toLocaleTimeString()
  messages.value.push(`[${time}] ${message}`)
}

// 中转站 2：接收 A 的"清空"请求，修改状态后 B 同步更新
function handleClear() {
  messages.value = []
}
</script>

<template>
  <div class="demo">
    <h2>兄弟组件通信演示</h2>
    <p class="intro">
      兄弟组件之间<strong>不能直接通信</strong>，需要借助共同的父组件中转：
      <code>A → emit → 父组件 → props → B</code>
    </p>

    <div class="parent">
      <h3>👨 父组件 SiblingDemo（数据中转站）</h3>
      <section class="block">
        <h4>父组件保管的共享状态</h4>
        <p>消息数量：<strong>{{ messages.length }}</strong></p>
        <p>
          父组件也可以直接改数据：
          <button class="btn" @click="messages = []">清空消息</button>
          <button
            class="btn secondary"
            @click="theme = theme === '#e67e22' ? '#9b59b6' : '#e67e22'"
          >
            切换 B 的主题色
          </button>
        </p>
      </section>
    </div>

    <div class="siblings">
      <!-- A 通过 emit 把数据交给父组件 -->
      <SiblingA @send="handleSend" @clear="handleClear" />
      <!-- 父组件再通过 props 把数据传给 B -->
      <SiblingB :messages="messages" :theme="theme" />
    </div>
  </div>
</template>

<style scoped>
.demo {
  max-width: 720px;
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

.siblings {
  display: flex;
  gap: 16px;
}

.hint {
  color: #888;
  font-size: 13px;
}
</style>
