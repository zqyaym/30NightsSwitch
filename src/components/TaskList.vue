<template>
  <div class="task-list">
    <div 
      v-for="task in tasks" 
      :key="task.id" 
      class="task-item"
      :class="{ 'task-item--completed': task.completed }"
    >
      <label class="task-checkbox">
        <input 
          type="checkbox" 
          :checked="task.completed"
          @change="toggleTask(task.id)"
        >
        <span class="checkmark"></span>
      </label>
      <span class="task-text">{{ task.text }}</span>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  tasks: {
    type: Array,
    required: true,
    default: () => []
  }
})

const emit = defineEmits(['task-toggle'])

const toggleTask = (taskId) => {
  emit('task-toggle', taskId)
}
</script>

<style scoped>
.task-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.task-item {
  display: flex;
  align-items: center;
  padding: 12px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
  transition: all 0.3s ease;
}

.task-item--completed {
  background-color: #F8F9FA;
  opacity: 0.7;
}

.task-item--completed .task-text {
  text-decoration: line-through;
  color: #6C757D;
}

.task-checkbox {
  position: relative;
  display: inline-block;
  width: 24px;
  height: 24px;
  margin-right: 12px;
  cursor: pointer;
}

.task-checkbox input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
}

.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  width: 24px;
  height: 24px;
  background-color: #fff;
  border: 2px solid #4B89DC;
  border-radius: 4px;
  transition: all 0.2s ease;
}

.task-checkbox input:checked ~ .checkmark {
  background-color: #4B89DC;
}

.checkmark:after {
  content: "";
  position: absolute;
  display: none;
  left: 8px;
  top: 4px;
  width: 5px;
  height: 10px;
  border: solid white;
  border-width: 0 2px 2px 0;
  transform: rotate(45deg);
}

.task-checkbox input:checked ~ .checkmark:after {
  display: block;
}

.task-text {
  font-size: 16px;
  color: #2C3E50;
  transition: all 0.3s ease;
}

.task-item:hover {
  transform: translateY(-1px);
  box-shadow: 0 2px 5px rgba(0,0,0,0.15);
}
</style>