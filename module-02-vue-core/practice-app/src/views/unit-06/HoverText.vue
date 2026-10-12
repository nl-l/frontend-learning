<script setup>
import { ref } from 'vue'

// 作业3：鼠标移上去文本样式变化，离开恢复
const hovering = ref(false)
</script>

<template>
  <div class="page">
    <h1>作业 3 · 鼠标悬停文本变色</h1>
    <p class="desc">把鼠标移到下面这段文本上，颜色、大小、粗细会变化；移开后恢复正常。</p>

    <p
      class="text"
      :class="{ active: hovering }"
      @mouseenter="hovering = true"
      @mouseleave="hovering = false"
    >
      Vue 的事件处理通过 v-on 指令（简写 @）完成。鼠标移上来试试：颜色变绿、字号变大、字体加粗；
      鼠标离开后，这段文字会恢复成原来的样子。
    </p>

    <p class="state">当前状态：{{ hovering ? '鼠标悬停中' : '鼠标已离开' }}</p>
  </div>
</template>

<style scoped>
.desc { color: #888; font-size: 13px; margin: 8px 0 20px; }
.text {
  max-width: 560px;
  padding: 16px;
  font-size: 15px;
  color: #333;
  border: 1px solid #eee;
  border-radius: 8px;
  cursor: default;
  transition: all 0.25s ease;
}
.text.active {
  color: #42b883;
  font-size: 19px;
  font-weight: 700;
  background: #f0faf5;
  border-color: #42b883;
}
.state { margin-top: 12px; font-size: 13px; color: #666; }
</style>
