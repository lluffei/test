<script setup>
import { ref, onMounted, nextTick } from 'vue'

let list = ref([])
let loading = ref(false)
let content = ref()

onMounted(() => {
  getList(10)
  window.addEventListener('scroll', handleScroll, true)
})

const getList = (num) => {
  loading.value = true
  if (!list.value.length) {
    setTimeout(() => {
      console.log('首次执行了')
      list.value = [{ background: 'rgb(233, 32, 38)' }]
      for (let i = 0; i < num - 1; i++) {
        list.value.push({
          background: `rgb(${Math.random() * 256}, ${Math.random() * 256}, ${
            Math.random() * 256
          })`,
        })
      }

      loading.value = false
    }, Math.random() * 5000)
  } else {
    setTimeout(() => {
      console.log('执行了')
      for (let i = 0; i < num; i++) {
        list.value.push({
          background: `rgb(${Math.random() * 256}, ${Math.random() * 256}, ${
            Math.random() * 256
          })`,
        })
      }

      loading.value = false
    }, Math.random() * 5000)
  }
}

// 节流
let flag = false
const handleScroll = () => {
  if (flag) return

  flag = true
  setTimeout(() => {
    flag = false
    let scroll = document.documentElement.scrollTop || document.body.scrollTop
    console.log(scroll)

    // 实现监听滚动条事件
    nextTick(() => {
      console.log(content.value.scrollHeight)
      if (scroll > content.value.scrollHeight / 2 && list.value.length < 50) {
        getList(10)
      }
    })
  }, 500)
}
</script>

<template>
  <div class="content" ref="content">
    <div class="box" v-for="(item, index) in list" :key="index" :style="item">
      {{ index }}
    </div>
  </div>
  <div class="loading" v-if="loading">正在加载...</div>
</template>

<style scoped>
.content {
  width: 100%;
}
.box {
  width: 100%;
  height: 500px;
  text-align: center;
  line-height: 500px;
}

.loading {
  width: 100%;
  height: 50px;
  text-align: center;
  line-height: 50px;
}
</style>
