<template>
  <div class="stopwatch-widget">
    <h2 class="widget-title">Widget Stopwatch</h2>
    <div class="timer">{{ formatTime(currentTime) }}</div>
    <div class="buttons">
      <button @click="toggleStartPause">
        {{ running ? "Berhenti" : "Mulai" }}
      </button>
      <button @click="reset">
        Reset
      </button>
      <button @click="recordLap">
        Sampel
      </button>
    </div>
    <div class="laps" v-if="laps.length > 0">
      <ul>
        <li v-for="(lap, index) in laps" :key="index">
          <span class="lap-index">Sampel {{ index + 1 }} : </span>
          <span class="lap-time">{{ formatTime(lap) }}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      running: false,
      currentTime: 0,
      intervalId: null,
      startTime: 0,
      laps: []
    };
  },
  computed: {
    formatTime() {
      return (time) => {
        const menit = Math.floor(time / 60000)
          .toString()
          .padStart(2, "0");
        const detik = Math.floor((time % 60000) / 1000)
          .toString()
          .padStart(2, "0");
        const milidetik = Math.floor((time % 1000) / 10)
          .toString()
          .padStart(2, "0");
        return `${menit}:${detik}.${milidetik}`;
      };
    }
  },
  methods: {
    toggleStartPause() {
      if (!this.running) {
        this.start();
      } else {
        this.pause();
      }
    },
    start() {
      if (!this.running) {
        this.running = true;
        this.startTime = Date.now() - this.currentTime;
        this.intervalId = setInterval(() => {
          this.currentTime = Date.now() - this.startTime;
        }, 10);
      }
    },
    pause() {
      if (this.running) {
        this.running = false;
        clearInterval(this.intervalId);
      }
    },
    reset() {
      this.running = false;
      clearInterval(this.intervalId);
      this.currentTime = 0;
      this.startTime = 0;
      this.laps = [];
    },
    recordLap() {
      if (this.running) {
        this.laps.push(this.currentTime);
      }
    }
  },
  beforeUnmount() {
    clearInterval(this.intervalId);
  }
};
</script>

<style scoped>
.stopwatch-widget {
  border: 1px solid #ccc;
  padding: 20px;
  margin-bottom: 20px;
  text-align: center;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.timer {
  font-size: 3rem;
  margin-bottom: 20px;
  color: #084d71;
  font-family: "Arial", sans-serif;
}

.buttons button {
  margin-right: 10px;
  font-size: 1.2rem;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  background-color: #084d71;
  color: #ffffff;
  cursor: pointer;
  transition: background-color 0.3s, color 0.3s;
}

.buttons button:hover {
  background-color: #98d4ff;
  color: #084d71;
}

.widget-title {
  margin-top: 0;
  color: #333;
  font-size: 24px;
}

.laps {
  margin-top: 20px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin-bottom: 5px;
}

.lap-index {
  font-weight: bold;
}

.lap-time {
  font-style: italic;
}
</style>
