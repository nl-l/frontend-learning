<script setup>
import { ref } from 'vue'

// 作业1：账号密码登录 —— 非空校验、密码不少于6位、回车提交、提示5秒后消失
const username = ref('')
const password = ref('')
const status = ref('') // '' | 'loading' | 'success' | error message

let timer = null

function submitLogin() {
  // 校验
  if (!username.value.trim() || !password.value) {
    status.value = '账号和密码不能为空'
    return
  }
  if (password.value.length < 6) {
    status.value = '密码不能少于 6 位'
    return
  }
  // 登录中提示，5 秒后变为成功提示
  status.value = 'loading'
  clearTimeout(timer)
  timer = setTimeout(() => {
    status.value = `success`
  }, 5000)
}
</script>

<template>
  <div class="page">
    <h1>作业 1 · 回车登录</h1>
    <p class="desc">账号、密码均非空，密码不少于 6 位；敲回车后显示「正在登录中」，5 秒后提示登录成功。</p>

    <!-- @keyup.enter 回车提交；按钮也可点击 -->
    <form class="form" @submit.prevent="submitLogin">
      <label>
        账号
        <input v-model.trim="username" type="text" placeholder="请输入账号" @keyup.enter="submitLogin" />
      </label>
      <label>
        密码
        <input v-model="password" type="password" placeholder="至少 6 位" @keyup.enter="submitLogin" />
      </label>
      <button type="submit">登录</button>
    </form>

    <p v-if="status === 'loading'" class="msg loading">正在登录中…</p>
    <p v-else-if="status === 'success'" class="msg success">{{ username }}（账号）登录成功</p>
    <p v-else-if="status" class="msg error">{{ status }}</p>
  </div>
</template>

<style scoped>
.page { max-width: 480px; }
.desc { color: #888; font-size: 13px; margin: 8px 0 20px; }
.form { display: flex; flex-direction: column; gap: 14px; }
label { display: flex; flex-direction: column; gap: 6px; font-size: 14px; }
input {
  padding: 10px 12px;
  border: 1px solid #ddd;
  border-radius: 6px;
  font-size: 14px;
}
input:focus { outline: 2px solid #42b88355; border-color: #42b883; }
button {
  padding: 10px;
  background: #42b883;
  color: #fff;
  border: none;
  border-radius: 6px;
  font-size: 15px;
  cursor: pointer;
}
button:hover { background: #35a06f; }
.msg { margin-top: 16px; padding: 10px 12px; border-radius: 6px; font-size: 14px; }
.loading { background: #fff8e1; color: #b8860b; }
.success { background: #e6f7ee; color: #2d8a5f; }
.error { background: #fdecea; color: #c0392b; }
</style>
