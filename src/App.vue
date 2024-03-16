<template>
  <div class="d-flex">
    <div class="app-wrapper col-xs-12 col-md-6 p-4">
      <div class="row justify-content-center">
        <div class="weather-container">
          <h1 class="mt-5">Weather App</h1>
          <input @keyup.enter="fetchWeather" class="br-1 w-100" type="text" placeholder="Enter location" v-model="country">
          <button class="btn btn-primary btn-large px-5 py-2 w-100" @click="fetchWeather">Get Weather</button>
          <div class="result-container bg-light br-1 w-100">
            <div v-if="weatherData && !error">
              <div v-for="(weather, index) in weatherData" :key="index" class="d-flex flex-column gap-3">
                <div class="d-flex align-items-center gap-2">
                  <span class="text-bold fs-5">Location:</span>
                  <span class="fs-5">{{weather.name}}, {{weather.sys.country}}</span>
                </div>
                <div class="d-flex align-items-center gap-2">
                  <span class="text-bold fs-5">Temperature:</span>
                  <span class="fs-5">{{ (weather.main.temp - 273.15).toFixed(0) }} °C</span>
                </div>
                <div class="d-flex align-items-center gap-2">
                  <span class="text-bold fs-5">Feels like:</span>
                  <span class="fs-5">{{ (weather.main.feels_like - 273.15).toFixed(0) }} °C</span>
                </div>
                <div class="d-flex align-items-center gap-2">
                  <span class="text-bold fs-5">Description:</span>
                  <span class="fs-5">{{weather.weather[0].main}}</span>
                </div>
              </div>
            </div>
            <div v-if="error" class="h-100 d-flex justify-content-center align-items-center flex-column">
              <p class="fs-5">
                Not found. Please try again
              </p>
              <div class="fs-5">
                <span class="text-success px-2">Example:</span>
                <span>Bristol, GB</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
export default {
  data() {
    return {
      country: '',
      apiKey: import.meta.env.VITE_APP_WEATHER_API_KEY,
      weatherData: null,
      error: null
    };
  },
  methods: {
    fetchWeather() {
      const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${this.country}&appid=${this.apiKey}`;

      fetch(apiUrl)
        .then(response => {
          if (!response.ok) {
            throw new Error('Network response was not ok');
          }
          return response.json();
        })
        .then(data => {
          this.weatherData = [data];
          this.error = null;
        })
        .catch(error => {
          this.error = error.message;
          console.error('There was a problem with the fetch operation:', error);
        });
    }
  },
}
</script>

<style scoped>

.app-wrapper {
  background-color: rgba(197, 236, 232, 0.5);
  height: 100vh;
  margin: 0 auto;
}

.weather-container {
    padding-top: 120px;
    height: 500px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    gap: 30px;
}

input {
    padding: 15px;
}

.result-container {
    border: 2px solid grey;
    min-height: 250px;
    padding: 30px;
}

.br-1{
  border-radius: 8px;
}

.text-bold {
  font-weight: 700;
}

@media only screen and (max-width: 600px) {

}

</style>