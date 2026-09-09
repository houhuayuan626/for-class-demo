<script setup lang="ts">
import { ref } from 'vue'
import LifecycleDemo from './components/LifecycleDemo.vue'

// 通过 v-if 控制组件的挂载 / 卸载，用来演示 onBeforeUnmount 和 onUnmounted
const show = ref<boolean>(true)
</script>

<template>
  <h1>grade-four-demo</h1>

  <button class="toggle" @click="show = !show">
    {{ show ? '卸载组件（演示卸载生命周期）' : '重新挂载组件' }}
  </button>

  <LifecycleDemo v-if="show" />
  <p v-else>组件已被卸载，点击上方按钮重新挂载。</p>
</template>

<style scoped>
h1 {
  text-align: center;
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
