<template>
  <div class="weather-widget">
    <h2 class="widget-title">Weather Widget</h2>
    <div class="location-input">
      <label for="location">Enter Location:</label>
      <input type="text" id="location" v-model="location" />
      <button class="get-weather-button" @click="fetchWeatherData">Get Weather</button>
    </div>
    <div v-if="weatherData" class="weather-data">
      <p class="location">Location: {{ weatherData.name }}</p>
      <p v-if="weatherData.main" class="temperature">
        Temperature: {{ Math.round(weatherData.main.temp) }}°C
      </p>
      <p v-if="weatherData.weather" class="description">
        Description: {{ weatherData.weather[0].description }}
      </p>
    </div>
    <p v-else class="loading-message">Loading weather data...</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      location: '',
      weatherData: null
    };
  },
  methods: {
    async fetchWeatherData() {
      try {
        const apiKey = 'b7bfca7b27a3485144fea086c50d09dc';
        const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${this.location}&appid=${apiKey}&units=metric`;

        const response = await fetch(apiUrl);
        const data = await response.json();

        this.weatherData = data;
      } catch (error) {
        console.error('Error fetching weather data:', error);
      }
    }
  }
};
</script>

<style scoped>
.weather-widget {
  border: 1px solid #ccc;
  padding: 20px;
  margin-bottom: 20px;
  text-align: center;
  background-color: #f2f2f2;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.widget-title {
  margin-top: 0;
  color: #333;
  font-size: 24px;
}

.location-input {
  margin-bottom: 10px;
}

.location-input label {
  font-size: 18px;
  color: #333;
}

.location-input input {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 16px;
}

.get-weather-button {
  padding: 10px 20px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.get-weather-button:hover {
  background-color: #45a049;
}

.weather-data {
  margin-top: 20px;
}

.location {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 10px;
}

.temperature {
  font-size: 36px;
  color: #ff5722;
  margin-bottom: 5px;
}

.description {
  font-size: 18px;
  margin-bottom: 10px;
}

.loading-message {
  color: #666;
  font-size: 16px;
  margin-top: 20px;
  font-style: italic;
}
</style>
