<template>
  <div>
    <!-- 插值 {{}} -->
    <p>姓名: {{ name }}</p>

    <!-- v‑bind 属性绑定 -->
    <img v-bind:src="imgUrl" alt="">

    <!-- v‑on事件绑定 @click -->
    <button @click="changeName">修改名字</button>

    <!-- v‑model双向绑定 -->
    <input v-model="name"/>

    <!-- v‑if 条件渲染 -->
    <p v-if="isShow">显示内容</p>

    <!-- v‑for 列表循环渲染，key用item.id！！ -->
    <ul>
      <li v-for="item in list" :key="item.id">{{ item.title }}</li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const name = ref("张三")
const imgUrl = ref("https://picsum.photos/100")
const isShow = ref(true)
const list = ref([
  {id:1, title:"Vue学习1"},
  {id:2, title:"Vue学习2"}
])

const changeName = ()=>{
  name.value = "李四"
}
</script>

<style>
</style>
