<template>
    <div class="h-screen w-screen content-center">
      <div class="second-bg">
        <div class="weather-content flex flex-col">
          <h1 class="text-center mt-3 rounded-full"><i>Weatherify</i></h1>
          <h2 class="text-center mt-5 mb-3">Enter a city name</h2>
          <div class="weather-form flex justify-center flex-col gap-5">
            <input type="text" id="inputt" class="w-full px-12 rounded-lg" placeholder="E.g. 'Almaty'" required>
            <!-- <button class="btn" @click="getWeather">Get current weather</button> -->
             <nuxt-link to="/"></nuxt-link>
          </div>
          <div>
            <slot />
          </div>
        </div>
      </div>
    </div>
</template>