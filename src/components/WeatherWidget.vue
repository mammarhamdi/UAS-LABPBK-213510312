<template>
  <div class="weather-widget">
    <h2 class="widget-title">Widget Cuaca</h2>
    <div class="location-input">
      <label for="location">Masukkan Lokasi : </label>
      <input type="text" id="location" v-model="location" />
      <button @click="fetchWeatherData">Dapatkan Cuaca</button>
    </div>
    <div v-if="weatherData" class="weather-data">
      <p class="location">Lokasi: {{ weatherData.name }}</p>
      <p v-if="weatherData.main" class="temperature">
        Suhu: {{ weatherData.main.temp }}°C
      </p>
      <p v-if="weatherData.weather" class="description">
        Kondisi: {{ weatherData.weather[0].description }}
      </p>
    </div>
    <p v-else>Sedang mengambil data cuaca...</p>
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
        const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${this.location}&appid=${apiKey}&units=metric&lang=id`;

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
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.widget-title {
  margin-top: 0;
  color: #333;
  font-size: 24px;
}

.location-input {
  margin-bottom: 20px;
}

.location-input label {
  font-size: 18px;
}

.location-input input {
  width: 200px;
  height: 30px;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
}

.location-input button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  background-color: #084d71;
  color: #fff;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s, color 0.3s;
}

.location-input button:hover {
  background-color: #333;
}

.weather-data {
  margin-top: 20px;
}

.location {
  font-size: 20px;
  font-weight: bold;
}

.temperature {
  font-size: 30px;
  color: #084d71;
  margin-top: 10px;
}

.description {
  font-size: 18px;
}
</style>
