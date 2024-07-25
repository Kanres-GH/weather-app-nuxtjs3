<template>
  <!-- <div class="h-screen w-screen content-center">
    <div class="second-bg">
      <div class="weather-content flex flex-col">
        <h1 class="text-center mt-3 rounded-full"><i>Weatherify</i></h1>
        <h2 class="text-center mt-5 mb-3">Enter a city name</h2>
        <div class="weather-form flex justify-center flex-col gap-5">
          <input type="text" id="inputt" class="w-full px-12 rounded-lg" placeholder="E.g. 'Almaty'" required>
          <button class="btn" @click="getWeather">Get current weather</button>
        </div>
        
      </div>
    </div>
  </div> -->
  
  </template>
  
  <script setup lang="ts">
    import { ref, onMounted } from 'vue';
  
    const inputElement = ref(null);
  
    onMounted(() => {
      inputElement.value = document.getElementById('inputt');

    });

    
  
    const getWeather = () => {
      
      const city = inputElement.value.value;

      // if(city === "") {
      //   console.log(1)
      //   btn.value.setAttribute("disabled", "disabled");
      //   butt.value.classList.add("btn-ds");
      // } else {
      //   butt.value.classList.remove("btn-ds");
      //   btn.value.removeAttribute("disabled");
      // }
  
      const { data: weather } = useFetch(`https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=6de4f249769d3afafcb3f7048aed7ac1&units=metric`)
  
      console.log(weather.value.main.temp);
  
      
  };
  
  
  // const { data: weather } = await useFetch(`https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=6de4f249769d3afafcb3f7048aed7ac1&units=metric`)
    
  
  
    
  </script>
  
  <style scoped>
    
  
  </style>
  <!-- API: 6de4f249769d3afafcb3f7048aed7ac1 -->
   <!-- Link: https://api.openweathermap.org/data/2.5/weather?q={CITY}&appid=6de4f249769d3afafcb3f7048aed7ac1&units=metric -->