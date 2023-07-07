<template>
  <div class="location-widget">
    <h2 class="widget-title">Widget Lokasi</h2>
    <div class="location-info" v-if="latitude && longitude">
      <div class="info-item">
        <p class="info-label">Masukkan Garis Lintang:</p>
        <p class="info-value">{{ latitude }}</p>
      </div>
      <div class="info-item">
        <p class="info-label">Masukkan Garis Bujur:</p>
        <p class="info-value">{{ longitude }}</p>
      </div>
    </div>
    <div v-else>
      <p class="loading-text">Mencari lokasi Anda...</p>
    </div>

    <div class="location-input">
      <label for="latitude" class="input-label">Masukkan Garis Lintang:</label>
      <input type="text" id="latitude" v-model="inputLatitude" class="input-field" />
    </div>
    <div class="location-input">
      <label for="longitude" class="input-label">Masukkan Garis Bujur:</label>
      <input type="text" id="longitude" v-model="inputLongitude" class="input-field" />
    </div>

    <button @click="fetchLocationDetails" class="find-button">Temukan Detail Lokasi</button>

    <div v-if="foundLocation" class="location-details">
      <h3>Detail Lokasi</h3>
      <p class="location-item">Negara: {{ foundLocation.components.country }}</p>
      <p class="location-item">Kota: {{ foundLocation.components.city }}</p>
      <p class="location-item">Jalan: {{ foundLocation.components.street }}</p>
      <p class="location-item">Kode Pos: {{ foundLocation.components.postcode }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      latitude: null,
      longitude: null,
      inputLatitude: '',
      inputLongitude: '',
      foundLocation: null,
    };
  },
  mounted() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(this.getPosition);
    }
  },
  methods: {
    getPosition(position) {
      this.latitude = position.coords.latitude;
      this.longitude = position.coords.longitude;
    },
    async fetchLocationDetails() {
      try {
        const apiKey = '92591005a7b94008909d59a64b6d2a49';
        const latitude = this.inputLatitude || this.latitude;
        const longitude = this.inputLongitude || this.longitude;
        const apiUrl = `https://api.opencagedata.com/geocode/v1/json?q=${encodeURIComponent(
          latitude + ',' + longitude
        )}&key=${apiKey}`;

        const response = await fetch(apiUrl);
        const data = await response.json();

        if (data.results && data.results.length > 0) {
          const location = data.results[0];
          this.foundLocation = location;
          console.log('Location:', location);
        }
      } catch (error) {
        console.error('Error fetching location data:', error);
      }
    },
  },
};
</script>

<style scoped>
.location-widget {
  background-color: #ffffff;
  border: 1px solid #e2e2e2;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 20px;
  margin-bottom: 20px;
  text-align: center;
}

.widget-title {
  color: #333333;
  font-size: 24px;
  margin-top: 0;
}

.location-info {
  margin-bottom: 20px;
}

.info-item {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 10px;
}

.info-label {
  color: #666666;
  font-size: 18px;
  margin-right: 10px;
}

.info-value {
  color: #333333;
  font-size: 18px;
}

.loading-text {
  color: #666666;
  font-size: 18px;
  font-style: italic;
}

.location-input {
  margin-top: 20px;
}

.input-label {
  color: #333333;
  display: block;
  font-size: 16px;
  margin-bottom: 5px;
}

.input-field {
  border: 1px solid #cccccc;
  border-radius: 4px;
  font-size: 16px;
  padding: 10px;
  width: 200px;
}

.find-button {
  background-color: #084d71;
  border: none;
  border-radius: 4px;
  color: #ffffff;
  cursor: pointer;
  font-size: 16px;
  margin-top: 20px;
  padding: 10px 20px;
  transition: background-color 0.3s;
}

.find-button:hover {
  background-color: #036592;
}

.location-details {
  margin-top: 20px;
  text-align: center;
}

.location-item {
  color: #333333;
  font-size: 18px;
  margin: 5px 0;
}
</style>
