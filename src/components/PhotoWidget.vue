<template>
  <div class="photo-widget">
    <h2 class="widget-title">{{ title }}</h2>
    <div class="photo-container">
      <img :src="photoUrl" :alt="title" />
    </div>
    <button class="load-button" @click="getRandomPhoto">Load New Photo</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: '',
      photoUrl: '',
    };
  },
  mounted() {
    this.getRandomPhoto();
  },
  methods: {
    async getRandomPhoto() {
      try {
        const apiKey = '38037020-2c48722c03be8437a05b588e6';
        const apiUrl = `https://pixabay.com/api/?key=${apiKey}&q=nature&image_type=photo&orientation=horizontal`;

        const response = await fetch(apiUrl);
        const data = await response.json();

        const randomIndex = Math.floor(Math.random() * data.hits.length);
        const randomPhoto = data.hits[randomIndex];

        this.title = randomPhoto.tags;
        this.photoUrl = randomPhoto.webformatURL;
      } catch (error) {
        console.error('Error fetching random photo:', error);
      }
    },
  },
};
</script>

<style scoped>
.photo-widget {
  border: 1px solid #ccc;
  padding: 20px;
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #f5f5f5;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.widget-title {
  color: #333;
  font-size: 24px;
  margin-bottom: 20px;
}

.photo-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  max-width: 500px;
  margin-top: 10px;
}

.photo-container img {
  max-width: 100%;
  max-height: 100%;
}

.load-button {
  margin-top: 10px;
  padding: 10px 20px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.load-button:hover {
  background-color: #45a049;
}

.load-button:disabled {
  background-color: #ccc;
  color: #999;
  cursor: not-allowed;
}
</style>
