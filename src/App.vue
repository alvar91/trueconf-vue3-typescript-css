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
