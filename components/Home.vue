<script setup lang="ts">
import { ref } from 'vue'

const city = ref('')
const weather = ref(null)
const backgroundImage = ref('')
const unsplashAccessKey = 'seVNJhx5ephCU9JvA2PMYcWtYMUo2gKR1mSrzj6HKZ4'

const getWeather = async () => {
  if (!city.value) return
  const apiKey = '4716a955503e2410adb8a31afecb0c1f'
  const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${city.value}&appid=${apiKey}&units=metric&lang=zh_tw`)
  weather.value = await response.json()
}

const BackgroundImage = async () => {
    const width = 1920
    const height = 1080
    const response = await fetch(`https://api.unsplash.com/photos/random?client_id=${unsplashAccessKey}&w=${width}&h=${height}`)
    const data = await response.json();
    console.log(data);
    const imageUrl = data.urls.regular
    backgroundImage.value = imageUrl
}

onMounted(() => {
  BackgroundImage()
})



</script>

<template>
<section :style="{ backgroundImage: `url(${backgroundImage})` }" class="warp">

  <div class="container">
    <h1>天氣查詢</h1>
    <div class="content">
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
  </div>

</section>
</template>

<style scoped>
.warp {
  height: 100vh;
  width: 100vw;
  padding: 0;
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
  background-color: #000;
}
.container {
  align-items: center;
  text-align: center;
}
.content {
  width: 400px;
  line-height: 100%;
  margin: auto;
}

input {
  padding: 0.5rem;
  margin-bottom: 1rem;
  width: 100%;
  max-width: 400px;
}


</style>
