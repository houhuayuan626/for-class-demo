<script setup lang="ts">
// SiblingB 是"接收方"：只负责通过 props 展示父组件转发来的数据
defineProps<{
  // 父组件转发过来的消息列表（原始来源是兄弟组件 A）
  messages: string[]
  // 父组件转发过来的主题色（原始来源同样是 A 的间接操作）
  theme: string
}>()
</script>

<template>
  <div class="sibling sibling-b" :style="{ borderColor: theme }">
    <h4 :style="{ color: theme }">🧒 兄弟组件 B（接收方）</h4>
    <p>
      收到 A 的消息：
      <span class="badge">{{ messages.length }} 条</span>
    </p>
    <ul class="msg-list">
      <li v-for="(msg, i) in messages" :key="i">{{ msg }}</li>
      <li v-if="!messages.length" class="hint">暂无消息，在 A 中发送一条试试</li>
    </ul>
    <p class="hint">B 的所有数据都来自父组件的 props，不直接接触 A</p>
  </div>
</template>

<style scoped>
.sibling {
  flex: 1;
  border: 2px dashed #e67e22;
  border-radius: 8px;
  padding: 12px 16px;
  background: #fffaf3;
  text-align: left;
}

.sibling h4 {
  margin: 0 0 8px;
  color: #e67e22;
}

.badge {
  display: inline-block;
  padding: 1px 8px;
  border-radius: 10px;
  background: #e67e22;
  color: #fff;
  font-size: 12px;
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

.hint {
  color: #888;
  font-size: 13px;
  margin-bottom: 0;
}
</style>
