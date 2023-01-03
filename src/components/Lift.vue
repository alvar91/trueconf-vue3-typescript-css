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
