

<template>
  <div class="app">
    <main>
      <h1>Weather App</h1>
      <p>Search by city, state, province or country, then press 'Enter':</p>
      <div class="search-box">
        <input type="text" name="" id="" class="search-bar" placeholder="search..." v-model="query"
          @keypress="fetchWeather">
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">
            {{ new Date().toDateString() }}
          </div>
          <div class="weather-box">
            <div class="temp">
              {{ Math.round(weather.main.temp) }}°C
            </div>
            <div class="weather">
              {{ weather.weather[0].main }}
              <span class="emoji">{{ getWeatherEmoji(weather.weather[0].main) }}</span>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'App',
  data() {
    return {
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.query}&units=metric&APPID=fce73cf6d09a525b7226f48bab5324a9`)
          .then(res => {
            this.weather = res.data;
            this.query = '';
            console.log(this.weather);
          })
          .catch(err => {
            console.log(err);
          })
      }
    },
    getWeatherEmoji(condition) {
      switch (condition) {
        case "Thunderstorm":
          return "⛈️";
        case "Drizzle":
          return "🌧️";
        case "Rain":
          return "☔";
        case "Snow":
          return "🌨️";
        case "Clear":
          return "🌞";
        case "Clouds":
          return "☁️";
        default:
          return "🌤️";
      }
    }
  }
}

</script>

