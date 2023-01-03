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
