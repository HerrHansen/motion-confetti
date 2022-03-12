<template>
  <div class="full-viewport">
    <div class="dummy-panel">
      <div class="dummy-row">
        <label>itemAmount:</label>
        <input
          type="number"
          v-model="config.itemAmount"
          @change="changeAmount"
        />
      </div>
      <div class="dummy-row">
        <label>next:</label>
        <input type="number" v-model="config.next" />
      </div>
      <div class="dummy-row">
        <label>duration min:</label>
        <input type="number" v-model="config.duration.min" />
      </div>
      <div class="dummy-row">
        <label>duration max:</label>
        <input type="number" v-model="config.duration.max" />
      </div>

      <div class="dummy-row">
        <div>
          <label>colors:</label>
          <div v-for="(color, index) in dummyColors" :key="index">
            <input
              type="checkbox"
              checked
              @change="toggleColor"
              :value="color"
            />
            {{ color }}
          </div>
        </div>
      </div>

      <div>
        in ♥️ with <a href="https://motion.dev">motion.dev</a>
      </div>
    </div>
    <Confetti :config="config" v-if="!isReloading" />
  </div>
</template>

<script setup>
import { ref } from "vue";
import Confetti from "./components/Confetti.vue";

const isReloading = ref(false);

const dummyColors = ["purple", "black", "red", "green", "orange", "yellow"];
const dummyStyles = [
  {
    width: "8px",
    height: "24px",
  },
  {
    width: "12px",
    height: "12px",
    borderRadius: "12px",
  },
  {
    width: "14px",
    height: "14px",
  },
];

const config = ref({
  itemAmount: 24,
  gap: 50,
  next: 0.5,
  duration: {
    min: 4,
    max: 6,
  },
  colors: [...dummyColors],
  styles: [...dummyStyles],
});

function toggleColor({ target }) {
  const index = config.value.colors.findIndex((c) => c === target.defaultValue);
  if (!target.checked) {
    if (index >= 0) config.value.colors.splice(index, 1);
  } else {
    if (index <= -1) config.value.colors.push(target.defaultValue);
  }
}

// TODO
function toggleStyle({ target }) {
  console.log("toggleColor", target.defaultValue);
  console.log("toggleColor", target.checked);
  const index = config.value.colors.findIndex((c) => c === target.defaultValue);
  if (!target.checked) {
    if (index >= 0) config.value.colors.splice(index, 1);
  } else {
    if (index <= -1) config.value.colors.push(target.defaultValue);
  }
}

function changeAmount() {
  isReloading.value = true;

  setTimeout(()=> isReloading.value = false, 200);
}
</script>

<style>
html, body {
  margin: 0;
}

.full-viewport {
  height: 100vh;
  width: 100vw;
}

html {
  font-family: sans-serif;
}

a {
  color: inherit;
  opacity: 0.6;
}
a:hover {
  opacity: 1;
}

.dummy-panel {
  position: relative;
  top: 30vh;
  z-index: 10;
  padding: 20px;
  margin: auto;
  width: 280px;
  background: white;
  border-radius: 10px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.5);
}

.dummy-row {
  margin-bottom: 10px;
}

input {
  width: 60px;
}

.dummy-row {
  display: flex;
  justify-content: space-between;
}
</style>
