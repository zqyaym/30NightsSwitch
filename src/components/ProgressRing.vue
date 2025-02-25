<template>
  <div class="progress-ring">
    <svg
      :width="size"
      :height="size"
      viewBox="0 0 100 100"
    >
      <circle
        class="progress-ring__circle-bg"
        :cx="center"
        :cy="center"
        :r="radius"
        :stroke-width="strokeWidth"
      />
      <circle
        class="progress-ring__circle"
        :cx="center"
        :cy="center"
        :r="radius"
        :stroke-width="strokeWidth"
        :stroke-dasharray="circumference"
        :stroke-dashoffset="dashOffset"
      />
      <text
        :x="center"
        :y="center"
        :dy="6"
        class="progress-ring__text"
      >
        第 {{ Math.round(progress / (100/30)) }} 天
      </text>
    </svg>
  </div>
</template>

<script setup>
import { computed, watch } from 'vue'
import { gsap } from 'gsap'

const props = defineProps({
  progress: {
    type: Number,
    required: true
  },
  size: {
    type: Number,
    default: 120
  }
})

const center = 50
const radius = 40
const strokeWidth = 8
const circumference = computed(() => 2 * Math.PI * radius)
const dashOffset = computed(() => 
  circumference.value - (props.progress / 100) * circumference.value
)

// 监听进度变化并添加动画
watch(() => props.progress, (newValue, oldValue) => {
  gsap.to('.progress-ring__circle', {
    duration: 0.5,
    ease: 'power2.out',
    strokeDashoffset: circumference.value - (newValue / 100) * circumference.value
  })
})
</script>

<style scoped>
.progress-ring {
  position: relative;
}

.progress-ring__circle-bg {
  fill: none;
  stroke: #E2E8F0;
}

.progress-ring__circle {
  fill: none;
  stroke: #4B89DC;
  transform: rotate(-90deg);
  transform-origin: center;
  transition: stroke-dashoffset 0.3s ease;
}

.progress-ring__text {
  fill: #2C3E50;
  font-size: 16px;
  text-anchor: middle;
  font-weight: bold;
}
</style>