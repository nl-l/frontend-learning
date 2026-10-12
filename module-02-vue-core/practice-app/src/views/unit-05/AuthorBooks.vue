<script setup>
import { reactive, computed } from 'vue'

// ── 原始数据（作业给定）──
const author = reactive({
  name: 'John Doe',
  books: [
    { title: 'Vue 2 - Advanced Guide', price: 99 },
    { title: 'Vue 3 - Basic Guide', price: 68 },
    { title: 'Vue 4 - The Mystery', price: 128 },
    { title: 'Java 21 - Basic Guide', price: 75 },
    { title: 'Java 21 - Advanced Guide', price: 110 }
  ]
})

// ── 作业 1：hasBooks —— 判断作者是否有书籍 ──
const hasBooks = computed(() => author.books.length > 0)

// ── 作业 2：vueBookList —— 筛选书名包含 "Vue" 的书籍 ──
const vueBookList = computed(() =>
  author.books.filter(b => b.title.includes('Vue'))
)

// ── 作业 3：vueBookTotalPrice —— Vue 相关书籍价格总和 ──
const vueBookTotalPrice = computed(() =>
  vueBookList.value.reduce((sum, b) => sum + b.price, 0)
)

// ── 作业 4：expensiveVueBooks —— Vue 书籍中价格 ≥ 100 ──
const expensiveVueBooks = computed(() =>
  vueBookList.value.filter(b => b.price >= 100)
)

// ── 作业 5：bookSummary —— 拼接汇总文本 ──
const bookSummary = computed(() =>
  `作者 ${author.name} 一共有 ${author.books.length} 本书，` +
  `其中 Vue 相关书籍共 ${vueBookList.value.length} 本，` +
  `Vue 书籍总价为 ${vueBookTotalPrice.value} 元`
)

// ── 交互：修改数据验证响应性 ──
function addBook() {
  author.books.push({ title: `New Book ${author.books.length + 1}`, price: 50 })
}
function changePrice() {
  const book = author.books[0]
  book.price = book.price === 99 ? 150 : 99
}
</script>

<template>
  <div class="author-books">
    <h1>Vue3 计算属性综合作业</h1>
    <p class="hint">修改数据后观察计算属性是否自动更新</p>

    <!-- 原始数据 -->
    <section>
      <h2>原始数据</h2>
      <table class="table">
        <thead>
          <tr><th>书名</th><th>价格</th></tr>
        </thead>
        <tbody>
          <tr v-for="b in author.books" :key="b.title">
            <td>{{ b.title }}</td>
            <td>{{ b.price }} 元</td>
          </tr>
        </tbody>
      </table>
    </section>

    <!-- 作业结果 -->
    <section>
      <h2>计算属性结果</h2>
      <div class="result">
        <p><b>1. hasBooks：</b><span class="bool">{{ hasBooks }}</span></p>
        <p><b>2. vueBookList：</b></p>
        <ul>
          <li v-for="b in vueBookList" :key="b.title">{{ b.title }} — {{ b.price }} 元</li>
        </ul>
        <p><b>3. vueBookTotalPrice：</b>{{ vueBookTotalPrice }} 元</p>
        <p><b>4. expensiveVueBooks（≥100）：</b></p>
        <ul>
          <li v-for="b in expensiveVueBooks" :key="b.title">{{ b.title }} — {{ b.price }} 元</li>
          <li v-if="expensiveVueBooks.length === 0">无</li>
        </ul>
        <p class="summary"><b>5. bookSummary：</b>{{ bookSummary }}</p>
      </div>
    </section>

    <!-- 交互按钮 -->
    <div class="actions">
      <button @click="addBook">添加书籍</button>
      <button @click="changePrice">切换第一本书价格 (99↔150)</button>
    </div>
  </div>
</template>

<style scoped>
.author-books h1 { font-size: 22px; margin-bottom: 8px; }
.hint { color: #888; font-size: 13px; margin-bottom: 20px; }
section { margin-bottom: 24px; }
h2 { font-size: 16px; margin-bottom: 12px; border-left: 3px solid #42b883; padding-left: 8px; }
.table { width: 100%; border-collapse: collapse; font-size: 14px; }
.table th, .table td { padding: 8px 12px; border-bottom: 1px solid #eee; text-align: left; }
.table th { background: #f5f5f5; }
.result p { margin-bottom: 8px; line-height: 1.8; }
.result ul { margin: 4px 0 12px 20px; }
.result li { font-size: 14px; color: #555; }
.bool { color: #42b883; font-weight: 600; }
.summary { background: #f0f9eb; padding: 12px 16px; border-radius: 8px; }
.actions { display: flex; gap: 12px; }
.actions button {
  padding: 8px 16px; border: 1px solid #42b883; border-radius: 6px;
  background: #fff; color: #42b883; cursor: pointer; font-size: 13px;
}
.actions button:hover { background: #42b883; color: #fff; }
</style>
