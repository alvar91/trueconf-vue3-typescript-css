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
