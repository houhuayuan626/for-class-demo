<script setup lang="ts">
import { ref } from 'vue'
import LifecycleDemo from './components/LifecycleDemo.vue'
import ParentChildDemo from './components/ParentChildDemo.vue'

// 当前展示的 demo：生命周期 / 父子通信
const activeDemo = ref<'lifecycle' | 'parent-child'>('parent-child')

// 通过 v-if 控制组件的挂载 / 卸载，用来演示 onBeforeUnmount 和 onUnmounted
const show = ref<boolean>(true)
</script>

<template>
  <h1>grade-four-demo</h1>

  <nav class="tabs">
    <button :class="{ active: activeDemo === 'parent-child' }" @click="activeDemo = 'parent-child'">
      父子组件通信
    </button>
    <button :class="{ active: activeDemo === 'lifecycle' }" @click="activeDemo = 'lifecycle'">
      生命周期函数
    </button>
  </nav>

  <ParentChildDemo v-if="activeDemo === 'parent-child'" />

  <template v-else>
    <button class="toggle" @click="show = !show">
      {{ show ? '卸载组件（演示卸载生命周期）' : '重新挂载组件' }}
    </button>

    <LifecycleDemo v-if="show" />
    <p v-else>组件已被卸载，点击上方按钮重新挂载。</p>
  </template>
</template>

<style scoped>
h1 {
  text-align: center;
}

.tabs {
  display: flex;
  justify-content: center;
  gap: 8px;
  margin-bottom: 20px;
}

.tabs button {
  padding: 8px 20px;
  border: 1px solid #42b883;
  border-radius: 6px;
  background: #fff;
  color: #42b883;
  cursor: pointer;
}

.tabs button.active {
  background: #42b883;
  color: #fff;
}

.toggle {
  display: block;
  margin: 0 auto 16px;
  padding: 6px 16px;
  border: 1px solid #42b883;
  border-radius: 6px;
  background: #fff;
  color: #42b883;
  cursor: pointer;
}

p {
  text-align: center;
  color: #888;
}
</style>
