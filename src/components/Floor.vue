<template>
  <div class="floor">
    <div class="button-container">
      <span class="title">{{ number }}</span>
      <button
        @click="$emit('call', number)"
        :class="['button', { activeButton: state.isRequested }]"
      >
        <span class="circle">
          <span class="dot"></span>
        </span>
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive, watch, onBeforeMount, toRefs } from "vue";

const props = defineProps({
  number: Number,
});

const state = reactive({
  isRequested: false,
});

watch(state, () => {
  localStorage.setItem(`floor-${props.number}`, JSON.stringify(state));
});

onBeforeMount(() => {
  const persistedJSON = localStorage.getItem(`floor-${props.number}`);

  if (persistedJSON !== null) {
    const { isRequested } = JSON.parse(persistedJSON);
    state.isRequested = isRequested;
  }
});

defineExpose({
  ...toRefs(props),
  ...toRefs(state),
});
</script>

<style scoped>
.floor {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  align-items: flex-start;
  justify-content: center;
  padding-left: 20px;
}

.button-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: var(--gap);
}

.button {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 30px;
  width: 30px;
  border: 1px solid var(--primary-color);
  border-radius: var(--border-radius);
  background-color: var(--light-color);
  cursor: pointer;
}

.circle {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 8px;
  height: 8px;
  border: 1px solid var(--primary-color);
  border-radius: 50%;
  padding: 2px;
}

.dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background-color: var(--primary-color);
}

.activeButton {
  border-color: var(--secondary-color);
}

.activeButton .circle {
  border-color: var(--secondary-color);
}

.activeButton .dot {
  background-color: var(--secondary-color);
}

.title {
  font-weight: 600;
}
</style>
