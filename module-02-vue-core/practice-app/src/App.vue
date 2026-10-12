<script setup>
import { ref } from 'vue'
import ActivityList from './views/unit-05/ActivityList.vue'

// 练习导航表，新增练习时在这里加一条
const practiceList = [
  { id: 'u05-activity', label: '单元 5 · 计算属性与缓存 — 活动列表', component: ActivityList }
]

const current = ref(practiceList[0])
</script>

<template>
  <div class="app">
    <aside class="sidebar">
      <h2>练习导航</h2>
      <ul>
        <li
          v-for="item in practiceList"
          :key="item.id"
          :class="{ active: current?.id === item.id }"
          @click="current = item"
        >
          {{ item.label }}
        </li>
      </ul>
    </aside>
    <main class="content">
      <component :is="current.component" v-if="current" />
    </main>
  </div>
</template>

<style>
* { margin: 0; padding: 0; box-sizing: border-box; }
body { font-family: system-ui, sans-serif; color: #333; }
.app { display: flex; min-height: 100vh; }
.sidebar {
  width: 280px;
  background: #f5f5f5;
  padding: 20px;
  border-right: 1px solid #ddd;
}
.sidebar h2 { font-size: 16px; margin-bottom: 16px; }
.sidebar ul { list-style: none; }
.sidebar li {
  padding: 10px 12px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  margin-bottom: 4px;
}
.sidebar li:hover { background: #e8e8e8; }
.sidebar li.active { background: #42b883; color: #fff; }
.content { flex: 1; padding: 24px; }
</style>
