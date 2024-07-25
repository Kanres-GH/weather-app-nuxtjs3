<template>
  <div class="h-screen w-screen content-center transition-colors duration-500" :class="backgroundClass">
    <div class="second-bg">
      <div class="weather-content flex flex-col">
        <h1 class="text-center mt-3 rounded-full antialiased"><i>Weatherify</i></h1>
        <h2 class="text-center mt-5 mb-3">Enter a city name</h2>
        <div class="weather-form flex justify-center flex-col gap-5">
          <input type="text" id="inputt" class="w-full px-12 rounded-lg" placeholder="E.g. 'Almaty'" required>
          <button class="btn" @click="getWeather">Get current weather</button>
        </div>
        <transition name="fade">
          <div v-if="weatherData" class="weather-result mt-3 p-5 bg-gray-500 rounded-lg shadow-lg transform transition-all duration-500 ease-in-out opacity-0 h-1/2 grid grid-cols-2 gap-4" :class="{ 'opacity-100': weatherData }">
            <div class="flex items-center col-span-2">
              <img :src="`https://openweathermap.org/img/wn/${weatherData.weather[0].icon}@4x.png`" class="w-24 h-24" alt="Weather Icon"/>
              <div class="ml-4">
                <h3 class="text-2xl font-bold antialiased">{{ weatherData.name }}</h3>
                <p class="text-xl mt-2">{{ weatherData.main.temp }}°C</p>
                <p class="text-lg">{{ weatherData.weather[0].description[0].toUpperCase() + weatherData.weather[0].description.substring(1) }}</p>
              </div>
            </div>
            <div class="flex flex-col items-center p-4 bg-gray-200 rounded-lg">
              <h3 class="text-xl mt-2">Feels Like</h3>
              <p>{{ weatherData.main.feels_like }}°C</p>
            </div>
            <div class="flex flex-col items-center p-4 bg-gray-200 rounded-lg">
              <h3 class="text-xl mt-2">Humidity</h3>
              <p>{{ weatherData.main.humidity }}%</p>
            </div>
            <div class="flex flex-col items-center p-4 bg-gray-200 rounded-lg">
              <h3 class="text-xl mt-2">Condition</h3>
              <p>{{ weatherData.weather[0].main }}</p>
            </div>
            <div class="flex flex-col items-center p-4 bg-gray-200 rounded-lg">
              <h3 class="text-xl mt-2">Wind Speed</h3>
              <p>{{ weatherData.wind.speed }} m/s</p>
            </div>
          </div>
        </transition>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue';

const inputElement = ref(null);
const weatherData = ref(null);

onMounted(() => {
  inputElement.value = document.getElementById('inputt');
});

const backgroundClass = computed(() => {
  if (!weatherData.value) return 'bg-gray-200';
  const temp = weatherData.value.main.temp;
  if (temp < 10) return 'bg-cyan-400';
  if (temp < 20) return 'bg-teal-500';
  return 'bg-amber-600';
});

const getWeather = async () => {
  const city = inputElement.value.value;
  const apiKey = '6de4f249769d3afafcb3f7048aed7ac1';
  const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`);
  weatherData.value = await response.json();
};
</script>

<style scoped>
.opacity-0 {
  opacity: 0;
}

.opacity-100 {
  opacity: 1;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter-from, .fade-leave-to {
  opacity: 0;
}

h3 {
  font-size: 1.8vw;
  font-weight: bold;
}

p {
  font-size: 1.5vw;
}
</style>
