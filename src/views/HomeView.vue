<template>
  <div class="home">
    <DailyQuote />
    <header class="header">
      <ProgressRing :progress="progress" />
      <div class="stats">
        <h2>第 {{ currentDay }} 天 / 30天</h2>
        <p>连续打卡: {{ streakDays }}天</p>
        <AchievementBadge :streak="streakDays" />
      </div>
    </header>

    <main class="main-content">
      <section class="task-section">
        <h3>今日任务</h3>
        <TaskList 
          :tasks="eveningTasks" 
          @task-toggle="handleTaskToggle"
        />
      </section>

      <section class="stats-section">
        <SleepChart :taskData="taskData" />
      </section>
    </main>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue'
import { useLocalStorage } from '@vueuse/core'
import ProgressRing from '../components/ProgressRing.vue'
import TaskList from '../components/TaskList.vue'
import SleepChart from '../components/SleepChart.vue'
import DailyQuote from '../components/DailyQuote.vue'
import AchievementBadge from '../components/AchievementBadge.vue'
import Clock from '../components/Clock.vue'

// 状态管理
const currentDay = useLocalStorage('currentDay', 1)
const streakDays = useLocalStorage('streakDays', 0)
const taskData = useLocalStorage('taskData', [])

// 计算进度
const progress = computed(() => (currentDay.value / 30) * 100)

// 晚间任务列表
const eveningTasks = useLocalStorage('eveningTasks', [
  { id: 1, text: '22:00 停用电子设备', completed: false },
  { id: 2, text: '热水泡脚', completed: false },
  { id: 3, text: '冥想/深呼吸', completed: false },
  { id: 4, text: '记录今日感想', completed: false },
])

// 更新任务数据
const updateTaskData = () => {
  const today = new Date().toDateString()
  const todayData = {
    date: today,
    tasks: [...eveningTasks.value]
  }
  
  const existingIndex = taskData.value.findIndex(data => data.date === today)
  if (existingIndex >= 0) {
    taskData.value[existingIndex] = todayData
  } else {
    taskData.value.push(todayData)
  }
}

// 检查是否需要重置任务状态
const resetTasksIfNeeded = () => {
  const lastActiveDate = localStorage.getItem('lastActiveDate')
  const today = new Date().toDateString()
  
  if (lastActiveDate !== today) {
    // 重置任务状态
    eveningTasks.value = eveningTasks.value.map(task => ({ ...task, completed: false }))
    
    // 更新天数和连续打卡
    const yesterdayTasks = taskData.value.find(data => data.date === lastActiveDate)?.tasks || []
    const allCompleted = yesterdayTasks.length > 0 && yesterdayTasks.every(task => task.completed)
    
    if (allCompleted) {
      streakDays.value++
      currentDay.value++
    } else {
      streakDays.value = 0
      currentDay.value = 1
    }
    
    localStorage.setItem('lastActiveDate', today)
  }
}

// 任务切换处理
const handleTaskToggle = (taskId) => {
  const task = eveningTasks.value.find(t => t.id === taskId)
  if (task) {
    task.completed = !task.completed
    updateTaskData()
  }
}

onMounted(() => {
  // 初始化逻辑
  resetTasksIfNeeded()
  updateTaskData()
})
</script>

<style scoped>
.home {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: #F8F9FA;
}

.header {
  display: flex;
  align-items: center;
  gap: 20px;
  padding: 20px;
  background-color: white;
  border-radius: 12px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.main-content {
  margin-top: 20px;
  display: grid;
  gap: 20px;
}

.task-section,
.stats-section,
.motivation-section {
  background-color: white;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

h2, h3 {
  color: #2C3E50;
  margin-bottom: 16px;
}
</style>
