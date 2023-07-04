<template>
  <div class="location-widget">
    <h2 class="widget-title">Your Location</h2>

    <div v-if="latitude && longitude" class="location-info">
      <p class="info-label">Latitude:</p>
      <p class="info-value">{{ latitude }}</p>
      <p class="info-label">Longitude:</p>
      <p class="info-value">{{ longitude }}</p>
    </div>

    <div v-else>
      <p class="loading-message">Finding your location...</p>
    </div>

    <div class="location-input">
      <label for="latitude">Latitude:</label>
      <input type="text" id="latitude" v-model="inputLatitude" placeholder="Enter latitude" />
    </div>

    <div class="location-input">
      <label for="longitude">Longitude:</label>
      <input type="text" id="longitude" v-model="inputLongitude" placeholder="Enter longitude" />
    </div>

    <button class="location-button" @click="fetchLocationDetails">Find Location Details</button>

    <div v-if="foundLocation" class="location-details">
      <h3>Location Details</h3>
      <p>{{ foundLocation.components.country }}</p>
      <p>{{ foundLocation.components.city }}</p>
      <p>{{ foundLocation.components.street }}</p>
      <p>Postal Code: {{ foundLocation.components.postcode }}</p>
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
          // Do something with the found location data
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
  border: 1px solid #ccc;
  padding: 20px;
  margin-bottom: 20px;
  text-align: center;
  background-color: #f5f5f5;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.widget-title {
  color: #333;
  font-size: 28px;
  margin-bottom: 20px;
}

.location-info {
  margin-bottom: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
}

.info-label {
  color: #666;
  font-size: 18px;
  margin-right: 10px;
}

.info-value {
  font-size: 18px;
  margin-bottom: 10px;
  font-weight: bold;
}

.loading-message {
  color: #666;
  font-size: 18px;
  margin-top: 10px;
}

.location-input {
  margin-top: 20px;
}

.location-input label {
  display: block;
  margin-bottom: 5px;
  color: #333;
  font-size: 18px;
}

.location-input input {
  width: 200px;
  padding: 10px;
  margin-right: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 16px;
}

.location-button {
  padding: 10px 20px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  margin-top: 10px;
}

.location-button:hover {
  background-color: #45a049;
}

.location-button:disabled {
  background-color: #ccc;
  color: #999;
  cursor: not-allowed;
}

.location-details {
  margin-top: 20px;
  text-align: left;
}

.location-details h3 {
  margin-bottom: 10px;
  color: #333;
  font-size: 18px;
}

.location-details p {
  margin: 5px 0;
  color: #666;
}

.error-message {
  color: red;
  margin-top: 10px;
}
</style>
