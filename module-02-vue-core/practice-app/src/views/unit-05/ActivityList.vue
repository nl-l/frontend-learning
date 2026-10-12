<script setup>
import { ref, computed } from 'vue'

// ── 模拟活动数据 ──
const activities = ref([
  { id: 1, name: '校园歌手大赛', status: 'signing', deadline: addDays(2) },
  { id: 2, name: '篮球联赛', status: 'signing', deadline: addDays(5) },
  { id: 3, name: '读书分享会', status: 'signing', deadline: addDays(3) },
  { id: 4, name: '编程挑战赛', status: 'signing', deadline: addDays(6) },
  { id: 5, name: '志愿者招募', status: 'signing', deadline: addDays(1) },
  { id: 6, name: '摄影展', status: 'signing', deadline: addDays(4) },
  { id: 7, name: '辩论赛', status: 'signing', deadline: addDays(7) },
  { id: 8, name: '创新创业讲座', status: 'signing', deadline: addDays(0) },
  { id: 9, name: '社团巡礼', status: 'signing', deadline: addDays(8) },
  { id: 10, name: '运动会', status: 'signing', deadline: addDays(6) },
  { id: 11, name: '电影之夜', status: 'signing', deadline: addDays(3) },
  { id: 12, name: '跳蚤市场', status: 'signing', deadline: addDays(5) },
  { id: 13, name: '迎新晚会', status: 'closed', deadline: addDays(-1) },
  { id: 14, name: '毕业展', status: 'closed', deadline: addDays(-3) },
  { id: 15, name: '校友论坛', status: 'ended', deadline: addDays(-5) },
  { id: 16, name: '开放日', status: 'ended', deadline: addDays(-7) },
  { id: 17, name: '新生军训', status: 'ended', deadline: addDays(-10) }
])

// 用于触发重新渲染的计数器，演示 methods 每次都重新执行
let methodCallCount = 0

// ── 方式一：普通函数（methods 写法）──
// 每次模板渲染都会重新执行，没有缓存
function totalCount() {
  methodCallCount++
  console.log(`[methods] totalCount 执行第 ${methodCallCount} 次`)
  return activities.value.length
}

function signingCount() {
  return activities.value.filter(a => a.status === 'signing').length
}

function closingThisWeek() {
  const now = new Date()
  const weekEnd = addDaysToDate(now, 7)
  return activities.value.filter(
    a => a.status === 'signing' && new Date(a.deadline) <= weekEnd
  ).length
}

// ── 方式二：计算属性（computed 写法）──
// 基于依赖缓存，activities 不变时不会重新计算
const totalCountComputed = computed(() => {
  console.log('[computed] totalCountComputed 重新计算')
  return activities.value.length
})

const signingCountComputed = computed(() => {
  console.log('[computed] signingCountComputed 重新计算')
  return activities.value.filter(a => a.status === 'signing').length
})

const closingThisWeekComputed = computed(() => {
  console.log('[computed] closingThisWeekComputed 重新计算')
  const now = new Date()
  const weekEnd = addDaysToDate(now, 7)
  return activities.value.filter(
    a => a.status === 'signing' && new Date(a.deadline) <= weekEnd
  ).length
})

// ── 切换两种模式 ──
const useComputed = ref(true)

// ── 修改数据触发重新渲染 ──
function addActivity() {
  activities.value.push({
    id: Date.now(),
    name: `新活动 ${activities.value.length + 1}`,
    status: 'signing',
    deadline: addDays(Math.floor(Math.random() * 10))
  })
}

function closeFirstSigning() {
  const target = activities.value.find(a => a.status === 'signing')
  if (target) target.status = 'closed'
}

// ── 触发无关重新渲染，观察 methods vs computed ──
const tick = ref(0)
function forceRender() {
  tick.value++
}

// ── 工具函数 ──
function addDays(n) {
  const d = new Date()
  d.setDate(d.getDate() + n)
  return d.toISOString()
}

function addDaysToDate(date, n) {
  const d = new Date(date)
  d.setDate(d.getDate() + n)
  return d
}

function formatDate(s) {
  return new Date(s).toLocaleDateString('zh-CN', { month: 'numeric', day: 'numeric' })
}
</script>

<template>
  <div class="activity-list">
    <h1>活动列表 · 计算属性与缓存</h1>

    <p class="hint">
      打开浏览器控制台观察输出。点击「触发无关渲染」后，
      methods 每次都会重新执行，而 computed 只有依赖变化时才重新计算。
    </p>

    <!-- 切换模式 -->
    <div class="toggle-bar">
      <label>
        <input type="radio" v-model="useComputed" :value="true" />
        computed 计算属性（有缓存）
      </label>
      <label>
        <input type="radio" v-model="useComputed" :value="false" />
        methods 普通函数（无缓存）
      </label>
    </div>

    <!-- 统计卡片 -->
    <div class="stats">
      <div class="stat-card">
        <span class="num">{{ useComputed ? totalCountComputed : totalCount() }}</span>
        <span class="label">活动总数</span>
      </div>
      <div class="stat-card">
        <span class="num">{{ useComputed ? signingCountComputed : signingCount() }}</span>
        <span class="label">报名中</span>
      </div>
      <div class="stat-card">
        <span class="num">{{ useComputed ? closingThisWeekComputed : closingThisWeek() }}</span>
        <span class="label">本周截止</span>
      </div>
    </div>

    <!-- 操作按钮 -->
    <div class="actions">
      <button @click="addActivity">新增活动</button>
      <button @click="closeFirstSigning">关闭首个报名中</button>
      <button @click="forceRender">触发无关渲染（tick={{ tick }}）</button>
    </div>

    <!-- 活动列表 -->
    <table class="table">
      <thead>
        <tr>
          <th>编号</th>
          <th>活动名称</th>
          <th>状态</th>
          <th>截止日期</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="a in activities" :key="a.id">
          <td>{{ a.id }}</td>
          <td>{{ a.name }}</td>
          <td>
            <span :class="['badge', a.status]">
              {{ a.status === 'signing' ? '报名中' : a.status === 'closed' ? '报名截止' : '已结束' }}
            </span>
          </td>
          <td>{{ formatDate(a.deadline) }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.activity-list h1 { font-size: 22px; margin-bottom: 12px; }
.hint {
  background: #fff8e1;
  padding: 12px 16px;
  border-radius: 8px;
  font-size: 13px;
  color: #795548;
  margin-bottom: 16px;
}
.toggle-bar {
  display: flex;
  gap: 24px;
  margin-bottom: 16px;
}
.toggle-bar label { font-size: 14px; cursor: pointer; }
.stats {
  display: flex;
  gap: 16px;
  margin-bottom: 20px;
}
.stat-card {
  flex: 1;
  background: #f8f9fa;
  border: 1px solid #e0e0e0;
  border-radius: 12px;
  padding: 24px;
  text-align: center;
}
.stat-card .num {
  display: block;
  font-size: 36px;
  font-weight: 700;
  color: #42b883;
}
.stat-card .label {
  display: block;
  font-size: 13px;
  color: #888;
  margin-top: 4px;
}
.actions {
  display: flex;
  gap: 12px;
  margin-bottom: 20px;
}
.actions button {
  padding: 8px 16px;
  border: 1px solid #42b883;
  border-radius: 6px;
  background: #fff;
  color: #42b883;
  cursor: pointer;
  font-size: 13px;
}
.actions button:hover { background: #42b883; color: #fff; }
.table {
  width: 100%;
  border-collapse: collapse;
  font-size: 14px;
}
.table th, .table td {
  padding: 10px 12px;
  text-align: left;
  border-bottom: 1px solid #eee;
}
.table th { background: #f5f5f5; font-weight: 600; }
.badge {
  padding: 2px 8px;
  border-radius: 4px;
  font-size: 12px;
}
.badge.signing { background: #e3f2fd; color: #1976d2; }
.badge.closed { background: #fff3e0; color: #f57c00; }
.badge.ended { background: #fafafa; color: #999; }
</style>
