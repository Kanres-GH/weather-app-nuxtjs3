<template>
  <div class="h-screen w-screen content-center transition-colors duration-500" :class="backgroundClass">
    <div class="second-bg">
      <div class="weather-content flex flex-col justify-self-center">
        <h1 class="text-center mt-3 rounded-full"><i>Weatherify</i></h1>
        <h2 class="text-center mt-5 mb-3">Enter a city name</h2>
        <div class="weather-form flex justify-center flex-col gap-5">
          <input
            type="text"
            id="inputt"
            v-model="city"
            @input="clearWarningMessage"
            class="w-full px-4 py-2 rounded-lg text-lg md:px-12 md:py-4"
            placeholder="E.g. 'Almaty'"
            required
          />
          <button
            class="btn"
            :disabled="!isInputValid"
            @click="getWeather"
          >
            Get current weather
          </button>
          <p v-if="warningMessage" class="text-red-500 text-center">{{ warningMessage }}</p>
        </div>
        <transition name="fade">
          <div
            v-if="weatherData"
            class="weather-result mt-3 p-3 bg-gray-500 rounded-lg shadow-lg transform transition-all duration-500 ease-in-out opacity-0 h-1/2 grid grid-cols-1 md:grid-cols-2 gap-4"
            :class="{ 'opacity-100': weatherData }"
          >
            <div class="flex items-center col-span-1 md:col-span-2">
              <img :src="`https://openweathermap.org/img/wn/${weatherData.weather[0].icon}@4x.png`" class="w-16 h-16 md:w-24 md:h-24" alt="Weather Icon" />
              <div class="ml-4">
                <h3 class="text-lg font-bold md:text-2xl">{{ weatherData.name }}</h3>
                <p class="text-base md:text-xl mt-1 md:mt-2">{{ weatherData.main.temp }}°C</p>
                <p class="text-sm md:text-lg">{{ weatherData.weather[0].description[0].toUpperCase() + weatherData.weather[0].description.substring(1) }}</p>
              </div>
            </div>
            <div class="flex flex-col items-center p-2 bg-gray-200 rounded-lg md:p-4">
              <i class="fas fa-temperature-low text-2xl md:text-4xl"></i>
              <h3 class="text-base md:text-xl mt-1 md:mt-2">Feels Like</h3>
              <p class="text-sm md:text-lg">{{ weatherData.main.feels_like }}°C</p>
            </div>
            <div class="flex flex-col items-center p-2 bg-gray-200 rounded-lg md:p-4">
              <i class="fas fa-tint text-2xl md:text-4xl"></i>
              <h3 class="text-base md:text-xl mt-1 md:mt-2">Humidity</h3>
              <p class="text-sm md:text-lg">{{ weatherData.main.humidity }}%</p>
            </div>
            <div class="flex flex-col items-center p-2 bg-gray-200 rounded-lg md:p-4">
              <i class="fas fa-cloud text-2xl md:text-4xl"></i>
              <h3 class="text-base md:text-xl mt-1 md:mt-2">Condition</h3>
              <p class="text-sm md:text-lg">{{ weatherData.weather[0].main }}</p>
            </div>
            <div class="flex flex-col items-center p-2 bg-gray-200 rounded-lg md:p-4">
              <i class="fas fa-wind text-2xl md:text-4xl"></i>
              <h3 class="text-base md:text-xl mt-1 md:mt-2">Wind Speed</h3>
              <p class="text-sm md:text-lg">{{ weatherData.wind.speed }} m/s</p>
            </div>
          </div>
        </transition>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const city = ref('');
const weatherData = ref(null);
const warningMessage = ref('');

const backgroundClass = computed(() => {
  if (!weatherData.value) return 'bg-gray-200';
  const temp = weatherData.value.main.temp;
  if (temp < 10) return 'bg-cyan-400';
  if (temp < 20) return 'bg-teal-500';
  return 'bg-amber-600';
});

const isInputValid = computed(() => {
  return city.value.trim() && !warningMessage.value;
});

const clearWarningMessage = () => {
  if (warningMessage.value) {
    warningMessage.value = '';
  }
};

const getWeather = async () => {
  // Validate input
  if (!city.value.trim()) {
    warningMessage.value = 'Please enter a city name.';
    return;
  }
  if (/[^a-zA-Z\s]/.test(city.value)) {
    warningMessage.value = 'City name can only contain letters and spaces.';
    return;
  }

  // Reset warning message
  warningMessage.value = '';

  try {
    const apiKey = '6de4f249769d3afafcb3f7048aed7ac1';
    const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${city.value}&appid=${apiKey}&units=metric`);
    
    if (!response.ok) {
      if (response.status === 404) {
        warningMessage.value = 'City not found. Please enter a valid city name.';
      } else {
        warningMessage.value = 'An error occurred. Please try again later.';
      }
      weatherData.value = null;
      return;
    }

    weatherData.value = await response.json();
  } catch (error) {
    warningMessage.value = 'An error occurred. Please try again later.';
  }
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
  font-size: 2vw;
  font-weight: bold;
}

p {
  font-size: 1.6vw;
}

.btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
</style>
