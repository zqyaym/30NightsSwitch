<template>
  <div class="sleep-chart">
    <h3>任务统计</h3>
    <div ref="chartContainer" class="chart-container"></div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch, computed } from 'vue'
import * as echarts from 'echarts'

const props = defineProps({
  taskData: {
    type: Array,
    required: true,
    default: () => []
  }
})

const chartContainer = ref(null)
let chart = null

// 获取最近7天的数据
const recentTaskData = computed(() => {
  return props.taskData.slice(-7)
})

// 计算每个任务的完成情况
const taskStats = computed(() => {
  return recentTaskData.value.map(dayData => {
    return dayData.tasks.reduce((acc, task) => {
      acc[task.text] = task.completed
      return acc
    }, {})
  })
})

const updateChart = () => {
  if (!chart) return

  const option = {
    tooltip: {
      trigger: 'axis',
      axisPointer: {
        type: 'shadow'
      },
      formatter: function(params) {
        const dayData = recentTaskData.value[params[0].dataIndex]
        if (!dayData) return '无数据'
        
        let result = `${dayData.date}<br/>`
        params.forEach(param => {
          const taskName = param.seriesName
          const completed = param.value
          result += `${taskName}: ${completed ? '已完成' : '未完成'}<br/>`
        })
        return result
      }
    },
    legend: {
      data: ['22:00 停用电子设备', '热水泡脚', '冥想/深呼吸', '记录今日感想'],
      bottom: 0,
      textStyle: {
        color: '#2C3E50'
      }
    },
    grid: {
      left: '3%',
      right: '4%',
      bottom: '15%',
      top: '10%',
      containLabel: true
    },
    color: ['#4B89DC', '#48C0A3', '#FFB236', '#FF7676'],
    xAxis: {
      type: 'category',
      data: recentTaskData.value.map(data => {
        const date = new Date(data.date)
        return `${date.getMonth() + 1}/${date.getDate()}`
      }),
      axisLabel: {
        color: '#2C3E50',
        interval: 0,
        rotate: 30
      }
    },
    yAxis: {
      type: 'value',
      name: '任务完成数量',
      min: 0,
      max: 4,
      interval: 1,
      axisLabel: {
        color: '#2C3E50'
      }
    },
    series: [
      '22:00 停用电子设备',
      '热水泡脚',
      '冥想/深呼吸',
      '记录今日感想'
    ].map(taskName => ({
      name: taskName,
      type: 'bar',
      stack: 'total',
      emphasis: {
        focus: 'series'
      },
      data: taskStats.value.map(day => day[taskName] ? 1 : 0)
    }))
  }
  
  chart.setOption(option)
}

const initChart = () => {
  if (!chartContainer.value) return
  
  chart = echarts.init(chartContainer.value)
  updateChart()
}

watch(() => props.taskData, () => {
  updateChart()
}, { deep: true })

onMounted(() => {
  initChart()
  window.addEventListener('resize', () => chart?.resize())
})
</script>

<style scoped>
.sleep-chart {
  width: 100%;
  background: #fff;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
}

.chart-controls {
  display: flex;
  gap: 12px;
  margin: 16px 0;
}

.metric-btn {
  padding: 8px 16px;
  border: 1px solid #4B89DC;
  border-radius: 20px;
  background: transparent;
  color: #4B89DC;
  cursor: pointer;
  transition: all 0.3s ease;
}

.metric-btn.active {
  background: #4B89DC;
  color: white;
}

.metric-btn:hover {
  background: #4B89DC15;
}

.chart-container {
  width: 100%;
  height: 300px;
  margin-top: 16px;
}

h3 {
  color: #2C3E50;
  margin: 0;
  font-size: 1.25rem;
}
</style>