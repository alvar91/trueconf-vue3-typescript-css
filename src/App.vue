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
