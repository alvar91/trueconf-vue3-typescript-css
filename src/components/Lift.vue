<template>
  <div class="shaft">
    <div
      :style="liftStyle"
      :class="['cabin', { blink: state.isOpen }]"
    >
      <span v-if="state.isMoving" :class="[{ up: state.isMovingUp }]"> ↓ </span>
      <span>{{ state.nextFloor }}</span>
    </div>
    <div v-for="floor in floors" :key="floor" class="cell"></div>
  </div>
</template>

<script setup lang="ts">
import { delay } from "@/utils/delay";
import { reactive, computed, watch, toRefs, onBeforeMount } from "vue";

const props = defineProps({
  number: Number,
  floors: Number,
});

const emit = defineEmits(["stop"]);

const state = reactive({
  currentFloor: 1,
  nextFloor: 1,
  isMoving: false,
  isMovingUp: false,
  isOpen: false,
  isIdle: true,
  timer: 3,
});

watch(
  () => state.nextFloor,
  () => {
    if (state.nextFloor !== state.currentFloor) {
      run();
    }
  }
);

function start() {
  state.isIdle = false;
  state.isMovingUp = state.currentFloor < state.nextFloor;
  state.isMoving = true;
}

async function stop() {
  emit("stop", state.currentFloor);

  state.isMoving = false;
  state.isOpen = true;

  while (state.timer > 0) {
    state.timer--;
    await delay(1000);
  }

  state.isOpen = false;
  state.isIdle = true;
  state.timer = 3;
}


async function run() {
  start();
  if (state.isMovingUp) {
    for (let i = state.currentFloor * 10, l = state.nextFloor * 10; i < l; await delay(100)) {
      i++;
      state.currentFloor = i / 10;
    }
  } else {
    for (let i = state.currentFloor * 10, l = state.nextFloor * 10; i > l; await delay(100)) {
      i--;
      state.currentFloor = i / 10;
    }
  }

  stop();
}

const liftStyle = computed(() => ({
  height: `${(1 / (props.floors ?? 1)) * 100}%`,
  transform: `translateY(-${(state.currentFloor - 1) * 100}%)`,
}));

watch(state, () => {
  localStorage.setItem(`lift-${props.number}`, JSON.stringify(state));
});

onBeforeMount(() => {
  const persistedJSON = localStorage.getItem(`lift-${props.number}`);

  if(persistedJSON !== null) {
    const {currentFloor, timer, isMoving, isMovingUp, isIdle, isOpen, nextFloor} = JSON.parse(persistedJSON);

    state.currentFloor = currentFloor;
    state.timer = timer;
    state.isMoving = isMoving;
    state.isMovingUp = isMovingUp;
    state.isIdle = isIdle;
    state.isOpen = isOpen;
    state.nextFloor = nextFloor;

    if (state.nextFloor === 1 && state.isMoving) run();

    if ((state.currentFloor === state.nextFloor && !state.isIdle) || state.isOpen) {
      setTimeout(() => stop(), 100);
    }
  }
});

defineExpose({
  ...toRefs(props),
  ...toRefs(state),
});
</script>

<style scoped>
.shaft {
  position: relative;
  display: flex;
  gap: var(--xs-gap);
  height: 100%;
  width: 100%;
  max-width: 80px;
  flex-direction: column;
  background-color: var(--grey-color);
}

.cabin {
  position: absolute;
  bottom: 0px;
  display: flex;
  width: 100%;
  align-items: center;
  justify-content: center;
  background-color: var(--primary-color);
  color: var(--light-color);
  font-weight: 600;
  transition: transform .3s linear;
}

.cell {
  width: 100%;
  flex-grow: 1;
  background-color: var(--light-color);
}

.up {
  transform: rotate(180deg);
}

.blink {
  animation: blink 0.5s infinite forwards alternate;
}

@keyframes blink {
  from {
    opacity: 1;
  }
  to {
    opacity: 0.4;
  }
}
</style>
