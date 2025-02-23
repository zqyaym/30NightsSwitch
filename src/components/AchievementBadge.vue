<template>
  <div class="achievement-badge" :class="badgeClass">
    <div class="badge-icon">🏆</div>
    <div class="badge-text">{{ badgeText }}</div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  streak: {
    type: Number,
    required: true
  }
})

const badgeClass = computed(() => {
  if (props.streak >= 30) return 'gold'
  if (props.streak >= 15) return 'silver'
  if (props.streak >= 7) return 'bronze'
  return 'starter'
})

const badgeText = computed(() => {
  if (props.streak >= 30) return '坚持达人'
  if (props.streak >= 15) return '稳步前进'
  if (props.streak >= 7) return '良好开端'
  return '开始征程'
})
</script>

<style scoped>
.achievement-badge {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 8px 12px;
  border-radius: 20px;
  font-size: 14px;
  font-weight: 600;
  margin-top: 8px;
  transition: all 0.3s ease;
}

.badge-icon {
  font-size: 18px;
}

.starter {
  background-color: #E2E8F0;
  color: #4A5568;
}

.bronze {
  background-color: #FBD38D;
  color: #975A16;
}

.silver {
  background-color: #CBD5E0;
  color: #2D3748;
}

.gold {
  background-color: #FCD34D;
  color: #92400E;
}

.achievement-badge:hover {
  transform: translateY(-2px);
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}
</style>