<template>
  <div class="h-screen w-screen content-center">
    <div class="second-bg">
      <div class="weather-content flex flex-col">
        <h1 class="text-center mt-3 rounded-full"><i>Weatherify</i></h1>
        <h2 class="text-center mt-5 mb-3">Enter a city name</h2>
        <div class="weather-form flex justify-center flex-col gap-5">
          <input type="text" id="inputt" class="w-full px-12 rounded-lg" placeholder="E.g. 'Almaty'" required>
          <button class="btn" @click="getWeather">Get current weather</button>
        </div>
        <div v-if="weatherData" class="weather-result mt-5 p-5 bg-gray-500 rounded-lg shadow-lg transform transition-all duration-500 ease-in-out opacity-0 h-1/2" :class="{ 'opacity-100': weatherData }">
        <h3 class="text-center text-2xl mb-4">Weather in {{ weatherData.name }}</h3>
        <p class="text-center">Temperature: {{ weatherData.main.temp }}°C</p>
        <p class="text-center">Feels like: {{ weatherData.main.feels_like }}°C</p>
        <p class="text-center">Humidity: {{ weatherData.main.humidity }}%</p>
        <p class="text-center">Condition: {{ weatherData.weather[0].main }}</p>
        <p class="text-center">Description: {{ weatherData.weather[0].description }}</p>
        <div class="flex justify-center">
          <img :src="`https://openweathermap.org/img/wn/${weatherData.weather[0].icon}@4x.png`" alt="Weather Icon"/>
        </div>
      </div>
      </div>

      
    </div>
  </div>
  
  </template>
  
  <script setup>
    import { ref, onMounted } from 'vue';
  
    const inputElement = ref(null);
    const weatherData = ref(null);
  
    onMounted(() => {
      inputElement.value = document.getElementById('inputt');
      
    });

  //   const getWeather = () => {
  //     const city = inputElement.value.value;
  //     let temperature, humidity, feels_like, cond, description, icon
  //     $fetch(`https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=6de4f249769d3afafcb3f7048aed7ac1&units=metric`)
  //     .then((weatherData) => {
  //       temperature = weatherData.main.temp
  //       humidity = weatherData.main.humidity
  //       feels_like = weatherData.main.feels_like
  //       cond = weatherData.weather.main
  //       description = weatherData.weather.description
  //       icon = `https://openweathermap.org/img/wn/${weatherData.weather.icon}@4x.png`
  //     });
  //    };
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
  
  </style>
  <!-- API: 6de4f249769d3afafcb3f7048aed7ac1 -->
   <!-- Link: https://api.openweathermap.org/data/2.5/weather?q={CITY}&appid=6de4f249769d3afafcb3f7048aed7ac1&units=metric -->