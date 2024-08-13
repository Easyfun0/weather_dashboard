<template>
  <div class="container">
    <h1>天氣儀表板</h1>
    <input v-model="city" @keyup.enter="getWeather" placeholder="輸入城市名稱" />
    <div v-if="weather && weather.main">
      <h2>{{ weather.name }} 的天氣</h2>
      <p>溫度：{{ weather.main.temp }}°C</p>
      <p>天氣狀況：{{ weather.weather[0].description }}</p>
      <p>濕度：{{ weather.main.humidity }}%</p>
      <p>風速：{{ weather.wind.speed }} m/s</p>
    </div>
    <p v-else-if="weather && weather.message">{{ weather.message }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const city = ref('')
const weather = ref(null)

const getWeather = async () => {
  if (!city.value) return
  const apiKey = '4716a955503e2410adb8a31afecb0c1f'
  const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${city.value}&appid=${apiKey}&units=metric&lang=zh_tw`)
  weather.value = await response.json()
}
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
  background-color: #fa0;
}

input {
  padding: 0.5rem;
  margin-bottom: 1rem;
  width: 100%;
  max-width: 400px;
}

h2 {
  margin-top: 2rem;
}
</style>


