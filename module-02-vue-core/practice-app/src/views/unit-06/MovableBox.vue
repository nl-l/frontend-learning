<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

// 作业2：方向键控制方块移动
const AREA_W = 560
const AREA_H = 320
const BOX = 40
const STEP = 10

const x = ref((AREA_W - BOX) / 2)
const y = ref((AREA_H - BOX) / 2)

function onKeydown(e) {
  const keys = ['ArrowUp', 'ArrowDown', 'ArrowLeft', 'ArrowRight']
  if (!keys.includes(e.key)) return
  e.preventDefault() // 阻止页面滚动
  if (e.key === 'ArrowUp') y.value = Math.max(0, y.value - STEP)
  if (e.key === 'ArrowDown') y.value = Math.min(AREA_H - BOX, y.value + STEP)
  if (e.key === 'ArrowLeft') x.value = Math.max(0, x.value - STEP)
  if (e.key === 'ArrowRight') x.value = Math.min(AREA_W - BOX, x.value + STEP)
}

// 键盘监听挂在 window 上，组件卸载时清理副作用
onMounted(() => window.addEventListener('keydown', onKeydown))
onUnmounted(() => window.removeEventListener('keydown', onKeydown))
</script>

<template>
  <div class="page">
    <h1>作业 2 · 方向键移动方块</h1>
    <p class="desc">点击本页面任意位置后，用键盘 ↑ ↓ ← → 控制方块移动（限制在活动区域内）。</p>

    <div class="area" :style="{ width: AREA_W + 'px', height: AREA_H + 'px' }">
      <div class="box" :style="{ left: x + 'px', top: y + 'px' }"></div>
    </div>
    <p class="pos">当前位置：x = {{ x }}，y = {{ y }}</p>
  </div>
</template>

<style scoped>
.desc { color: #888; font-size: 13px; margin: 8px 0 20px; }
.area {
  position: relative;
  border: 2px dashed #ccc;
  border-radius: 8px;
  background: #fafafa;
}
.box {
  position: absolute;
  width: 40px;
  height: 40px;
  background: #42b883;
  border-radius: 6px;
  transition: left 0.05s, top 0.05s;
}
.pos { margin-top: 12px; font-size: 13px; color: #666; }
</style>
