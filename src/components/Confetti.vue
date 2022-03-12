<template>
  <div class="confetti">
    {{ viewWidth }} x {{ viewHeight }}
    <Motion
      v-for="item in items"
      class="confetti-item"
      :key="item.id"
      :animate="item.animate"
      :transition="item.transition"
      :style="{ ...item.style, background: item.color }"
    />
  </div>
</template>

<script setup>
import { Motion } from "motion/vue";
import { onMounted, ref } from "vue";

const props = defineProps({
  config: {
    type: Object,
    default: {
      itemAmount: 10,
      gap: 50,
      next: 0.5,
      duration: {
        min: 4,
        max: 6,
      },
      colors: ["purple", "black", "red", "green", "orange", "yellow"],
      styles: [
        {
          width: "8px",
          height: "20px",
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
      ],
    },
  },
});

const viewWidth = ref(0);
const viewHeight = ref(0);

function initSize() {
  let box = document.querySelector(".confetti");
  viewHeight.value = box.offsetHeight;
  viewWidth.value = box.offsetWidth;
}

const items = ref([]);

function getRandomInt({ min, max }) {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.floor(Math.random() * (max - min)) + min;
}

function getRandomColor() {
  const colorIndex =
    Math.floor(Math.random() * props.config.colors.length) || 0;
  return props.config.colors[colorIndex];
}

function getRandomStyle() {
  const styleIndex =
    Math.floor(Math.random() * props.config.styles.length) || 0;
  const style = props.config.styles[styleIndex];
  return style;
}

function createItems() {
  items.value = [];
  for (let i = 0; i <= props.config.itemAmount; i++) {
    createItem(i);
  }
}

function createItem(i) {
  const duration = getRandomInt(props.config.duration);
  const easing = "ease-in";

  const id = `item-${i}`;

  const item = {
    id,
    style: getRandomStyle(),
    color: getRandomColor(),
    animate: {
      x: [
        getRandomInt({ min: 0, max: viewWidth.value }),
        getRandomInt({ min: 0, max: viewWidth.value }),
      ],
      y: [
        getRandomInt({ min: props.config.gap * -1, max: -100 }),
        viewHeight.value + props.config.gap,
      ],
      rotate: [
        getRandomInt({ min: -180, max: 180 }),
        getRandomInt({ min: -180, max: 180 }),
      ],
    },
    transition: { duration, easing },
    exit: { opacity: 0, transition: { duration: 0.8 } },
  };

  items.value.push(item);

  setTimeout(
    () => createItem(i + 1),
    getRandomInt({
      min: duration * 1000 * props.config.next,
      max: duration * 1000,
    }) * 0.5
  );

  setTimeout(() => {
    const index = items.value.findIndex((i) => i.id === id);
    if (index >= 0) items.value.splice(i, 1);
  }, duration * 1000 + 5000);
}

onMounted(() => {
  initSize();
  createItems();
});

window.onresize = initSize;
</script>

<style scoped>
.confetti {
  position: absolute;
  top: 0;
  left: 0;
  overflow: hidden;
  width: 100%;
  height: 100%;
}

.confetti-item {
  position: absolute;
  top: 0;
  left: 0;
}
</style>
