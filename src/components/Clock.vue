<template>
  <div class="clock">
    <div class="clock-container">
      <div class="date">{{ currentDate }}</div>
      <div class="time">{{ currentTime }}</div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const currentTime = ref('')
const currentDate = ref('')
let timer = null

const updateTime = () => {
  const now = new Date()
  const hours = now.getHours().toString().padStart(2, '0')
  const minutes = now.getMinutes().toString().padStart(2, '0')
  const seconds = now.getSeconds().toString().padStart(2, '0')
  currentTime.value = `${hours}:${minutes}:${seconds}`
  
  const year = now.getFullYear()
  const month = (now.getMonth() + 1).toString().padStart(2, '0')
  const day = now.getDate().toString().padStart(2, '0')
  currentDate.value = `${year}年${month}月${day}日`
}

onMounted(() => {
  updateTime()
  timer = setInterval(updateTime, 1000)
})

onUnmounted(() => {
  if (timer) {
    clearInterval(timer)
  }
})
</script>

<style scoped>
.clock {
  text-align: left;
  margin-bottom: 10px;
  padding: 0 20px;
}

.clock-container {
  display: flex;
  align-items: center;
  gap: 15px;
}

.date, .time {
  font-size: 1.3rem;
  font-weight: bold;
  color: #2C3E50;
}
</style>