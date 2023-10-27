<template>
  <header>
    Score: <span class="clickedScore"> {{ clicked }} </span> :
    <span class="missedScore"> {{ miss }}</span>
  </header>
  <main>
    <div v-if="!gameStarted" class="target-size">
      <input
        style="z-index: 99"
        type="range"
        min="10"
        max="120"
        v-model="size"
      />
      <div
        class="target-prew"
        :style="{ width: size + 'px', height: size + 'px' }"
      ></div>
    </div>
    <button v-if="!gameStarted && time !== 0" class="resume" @click="resume">
      DevamKe
    </button>
    <div v-if="!gameStarted" class="gameStart">
      <button @click="startGame(10)">10</button>
      <button @click="startGame(15)">15</button>
      <button @click="startGame(30)">30</button>
      <button @click="startGame(60)">60</button>
      <button @click="startGame(90)">90</button>
      <button @click="startGame(120)">120</button>
    </div>
    <div class="countdown">{{ time }}</div>
    <div class="screen-bg" @mousedown="missClick"></div>
    <div
      v-if="gameStarted"
      class="target"
      @mousedown="click"
      :style="{
        left: px + '%',
        top: py + '%',
        width: size + 'px',
        height: size + 'px',
      }"
    ></div>
  </main>
</template>
<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  name: "App",
  data() {
    return {
      gameStarted: false,
      score: 0,
      clicked: 0,
      miss: 0,
      px: 50,
      py: 50,
      time: 0,
      isFirstClick: true,
      size: 100,
    };
  },
  mounted() {
    // key listen is esc
    window.addEventListener("keyup", (e) => {
      if (e.key === "Escape") {
        this.gameStarted = false;
      }
    });
  },
  methods: {
    startGame(time: number) {
      this.gameStarted = true;
      this.clicked = 0;
      this.miss = 0;
      this.time = time;
      this.isFirstClick = true;
    },
    startInterval() {
      const interval = setInterval(() => {
        if (this.gameStarted) {
          this.time--;
        }
        if (this.time === 0) {
          clearInterval(interval);
          this.gameStarted = false;
        }
      }, 1000);
    },
    click() {
      if (this.isFirstClick) {
        this.startInterval();
        this.isFirstClick = false;
        this.clicked--;
      }
      if (this.gameStarted) {
        this.clicked++;
        this.px = Math.floor(Math.random() * 90);
        this.py = Math.floor(Math.random() * 90);
      }
    },
    missClick() {
      if (this.gameStarted && !this.isFirstClick) {
        this.miss++;
      }
    },
    resume() {
      this.gameStarted = true;
    },
  },
});
</script>

<style>
header {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 0;
  width: 100%;
  z-index: 10;
}
main {
  display: flex;
  height: 100vh;
  width: 100vw;
  z-index: 1;
}

.screen-bg {
  height: 100%;
  width: 100%;
  z-index: 1;
  position: absolute;
  background-color: var(--color-background);
}

.target {
  height: 100px;
  width: 100px;
  background-color: red;
  position: relative;
  display: flex;
  z-index: 999;
  border-radius: 8px;
}
.target:hover {
  border: 2px solid green;
}
.countdown {
  position: absolute;
  top: 10%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: white;
  font-size: 60px;
  font-weight: bold;
  z-index: 999;
}
.clickedScore {
  color: green;
  font-size: 24px;
  font-weight: bold;
  padding: 8px;
}
.missedScore {
  color: red;
  font-size: 24px;
  font-weight: bold;
  padding: 8px;
}
.gameStart {
  position: absolute;
  top: 50%;
  left: 5%;
  transform: translate(-50%, -50%);
  z-index: 999;
}
.gameStart button {
  padding: 8px;
  font-size: 24px;
  font-weight: bold;
  border-radius: 8px;
  margin: 8px;
  cursor: pointer;
  color: white;
  background-color: green;
  display: flex;
  flex-direction: column;
}
.resume {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 999;
  padding: 8px;
  font-size: 24px;
  font-weight: bold;
  border-radius: 8px;
  margin: 8px;
  cursor: pointer;
  color: black;
  background-color: white;
}
.target-size {
  position: absolute;
  right: 5%;
  top: 50%;
  z-index: 999;
  display: flex;
  flex-direction: column;
  gap: 100px;
}
.target-size .target-prew{
  background-color: red;
}
</style>
