<template>
  <div class="daily-quote">
    <h3>今日激励</h3>
    <Clock />
    <div class="quote-content">
      {{ currentQuote }}
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import Clock from './Clock.vue'

const quotes = [
  "良好的作息是健康生活的基石",
  "今天的坚持，是明天的收获",
  "规律作息，让生活更有质量",
  "早睡早起，精神百倍",
  "健康的生活方式从现在开始",
  "保持好的作息，让身体更有活力",
  "每一个好习惯都是对自己的投资",
  "坚持规律作息，让生活更有规律",
  "好的睡眠是最好的养生",
  "养成好习惯，收获好生活"
]

const currentQuote = ref(quotes[Math.floor(Math.random() * quotes.length)])
</script>

<style scoped>
.daily-quote {
  padding: 16px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.quote-content {
  font-size: 18px;
  color: #4B89DC;
  text-align: left;
  margin-top: 12px;
  padding: 0 20px;
  font-style: italic;
  line-height: 1.5;
}

h3 {
  color: #2C3E50;
  margin-bottom: 16px;
}
</style>