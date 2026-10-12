<script setup>
import { ref } from 'vue'
import ActivityList from './views/unit-05/ActivityList.vue'
import AuthorBooks from './views/unit-05/AuthorBooks.vue'
import LoginForm from './views/unit-06/LoginForm.vue'
import MovableBox from './views/unit-06/MovableBox.vue'
import HoverText from './views/unit-06/HoverText.vue'

// 练习导航表，新增练习时在这里加一条
const practiceList = [
  { id: 'u05-activity', label: '单元 5 · 计算属性与缓存 — 活动列表', component: ActivityList },
  { id: 'u05-author-books', label: '单元 5 · 计算属性综合作业 — 作者书籍', component: AuthorBooks },
  { id: 'u06-login', label: '单元 6 · 作业 1 回车登录', component: LoginForm },
  { id: 'u06-movable-box', label: '单元 6 · 作业 2 方向键移动方块', component: MovableBox },
  { id: 'u06-hover-text', label: '单元 6 · 作业 3 鼠标悬停文本', component: HoverText }
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
