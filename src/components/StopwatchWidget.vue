<template>
  <div class="stopwatch-widget">
    <h2 class="widget-title">Stopwatch</h2>
    <p class="time">{{ formatTime }}</p>
    <div class="button-container">
      <button class="start-button" @click="startStopwatch" :disabled="isRunning">Start</button>
      <button class="stop-button" @click="stopStopwatch" :disabled="!isRunning">Stop</button>
      <button class="reset-button" @click="resetStopwatch">Reset</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isRunning: false,
      startTime: null,
      elapsedTime: 0,
    };
  },
  computed: {
    formatTime() {
      const milliseconds = this.elapsedTime % 1000;
      const seconds = Math.floor(this.elapsedTime / 1000) % 60;
      const minutes = Math.floor(this.elapsedTime / 60000) % 60;
      const hours = Math.floor(this.elapsedTime / 3600000);

      return `${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}.${milliseconds.toString().padStart(3, '0')}`;
    },
  },
  methods: {
    startStopwatch() {
      if (!this.isRunning) {
        this.isRunning = true;
        this.startTime = Date.now();

        this.timerInterval = setInterval(() => {
          this.elapsedTime = Date.now() - this.startTime;
        }, 10);
      }
    },
    stopStopwatch() {
      if (this.isRunning) {
        this.isRunning = false;
        clearInterval(this.timerInterval);
      }
    },
    resetStopwatch() {
      this.isRunning = false;
      clearInterval(this.timerInterval);
      this.elapsedTime = 0;
    },
  },
};
</script>

<style scoped>
.stopwatch-widget {
  border: 1px solid #ddd;
  padding: 20px;
  margin-bottom: 20px;
  border-radius: 8px;
  background-color: #f8f8f8;
}

.widget-title {
  color: #333;
  font-size: 28px;
  margin-bottom: 10px;
}

.time {
  color: #666;
  font-size: 48px;
  margin-bottom: 10px;
}

.button-container {
  display: flex;
  justify-content: center;
  gap: 10px;
}

.start-button,
.stop-button,
.reset-button {
  padding: 12px 24px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.start-button:hover,
.stop-button:hover,
.reset-button:hover {
  background-color: #45a049;
}

.stop-button:disabled,
.reset-button:disabled {
  background-color: #ccc;
  color: #999;
  cursor: not-allowed;
}
</style>
