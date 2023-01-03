<template>
  <div class="layout">
    <Settings
      :floors="state.floors"
      :lifts="state.lifts"
      @submit="changeSettings"
    />
    <div class="container">
      <Lift
        v-for="lift in state.lifts"
        ref="liftList"
        :key="lift"
        :number="lift"
        :floors="state.floors"
        @stop="stop"
      />
      <div class="sub-container">
        <Floor
          v-for="floor in state.floors"
          ref="floorList"
          :key="floor"
          :number="floor"
          @call="call"
        />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, watchEffect, watch, onBeforeMount } from "vue";
import Settings from "./components/Settings.vue";
import Lift from "./components/Lift.vue";
import Floor from "./components/Floor.vue";
import { LiftI } from "./types/LiftI";
import { FloorI } from "./types/FloorI";
import { StateI } from "./types/StateI";

const liftList = ref<LiftI[]>([]);
const floorList = ref<FloorI[]>([]);

const state = reactive<StateI>({
  lifts: 1,
  floors: 5,
  queue: [],
});

onBeforeMount(() => {
  const persistedJSON = localStorage.getItem("app");

  if (persistedJSON !== null) {
    const { lifts, floors, queue } = JSON.parse(persistedJSON);
    state.lifts = lifts;
    state.floors = floors;
    state.queue = queue;
  }
});

watchEffect(() => {
  if (state.queue.length && liftList.value.some((lift) => lift.isIdle)) run();
});

watch(state, () => {
  localStorage.setItem("app", JSON.stringify(state));
});

function changeSettings(floor: number, lift: number) {
  localStorage.clear();

  state.floors = floor;
  state.lifts = lift;
  state.queue.length = 0;

  setTimeout(() => {
    window.location.reload();
  }, 100);
}

function call(floor: number) {
  if (
    !liftList.value.some((item) => item.currentFloor === floor && item.isIdle) &&
    !liftList.value.some((item) => item.nextFloor === floor) &&
    !state.queue.includes(floor)
  ) {
    const i = floorList.value.findIndex((item) => item.number === floor);
    floorList.value[i].isRequested = true;
    state.queue.push(floor);
  }
}

function stop(floor: number) {
  const i = floorList.value.findIndex((item) => item.number === floor);
  floorList.value[i].isRequested = false;
}

function run() {
  const freeLiftsArr = liftList.value.filter((item) => item.isIdle);

  const floorsOfFreeElevsArr = freeLiftsArr.map((item) => item.currentFloor).sort((a, b) => b - a);

  const closestFloor = floorsOfFreeElevsArr.reduce((a, b) =>
    Math.abs(b - state.queue[0]) < Math.abs(a - state.queue[0]) ? b : a
  );

  const i = liftList.value.findIndex((item) => item.currentFloor === closestFloor && item.isIdle);

  if (i !== -1 && state.queue.length > 0) {
    // eslint-disable-next-line
    liftList.value[i].nextFloor = state.queue.shift()!;
  }
}
</script>

<style>
:root {
  --primary-color: #00feff;
  --secondary-color: #ef7317;
  --light-color: #fff;
  --dark-color: #000;
  --grey-color: #e3e3e3;

  --border-radius: 5px;
  --xs-gap: 1px;
  --gap: 10px;
}

body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: var(--dark-color);
  box-sizing: border-box;
  background-color: var(--grey-color);
  user-select: none;
}

.layout {
  padding: var(--gap);
  height: 90vh;
}

.container {
  display: flex;
  margin-top: var(--gap);
  gap: var(--xs-gap);
  height: 100%;
  flex-direction: row;
  background-color: var(--grey-color);
}

.sub-container {
  display: flex;
  flex-grow: 1;
  flex-direction: column-reverse;
}
</style>
