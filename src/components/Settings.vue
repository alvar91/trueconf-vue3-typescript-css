<template>
  <button @click="open" class="button">Конфигурация</button>

  <Teleport to="#app">
    <Transition name="modal">
      <div v-if="state.isOpen" @click.self="close" class="modal">
        <div class="modal-container">
          <div class="modal-sub-container">
            <h2 class="modal-title">Конфигурация</h2>
            <button @click="close" class="close-button">×</button>
          </div>
          <div>
            <div class="modal-sub-container">
              <label for="floors">Этажи</label>
              <div class="controls">
                <button
                  @click="state.floorsInput--"
                  :disabled="+state.floorsInput <= 1"
                  class="button"
                >
                  -
                </button>
                <input
                  id="floors"
                  type="number"
                  required
                  v-model.number="state.floorsInput"
                  disabled
                />
                <button
                  @click="state.floorsInput++"
                  :disabled="+state.floorsInput >= 10"
                  class="button"
                >
                  +
                </button>
              </div>
            </div>
            <div class="modal-sub-container">
              <label for="lifts">Лифты</label>
              <div class="controls">
                <button
                  @click="state.liftsInput--"
                  :disabled="+state.liftsInput <= 1"
                  class="button"
                >
                  -
                </button>
                <input
                  id="lifts"
                  type="number"
                  required
                  v-model.number="state.liftsInput"
                  disabled
                />
                <button
                  @click="state.liftsInput++"
                  :disabled="+state.liftsInput >= 10"
                  class="button"
                >
                  +
                </button>
              </div>
            </div>
          </div>
          <div class="modal-sub-container">
            <button @click="close" class="button">Отмена</button>
            <button @click="submit" :disabled="!isSubmitEnable" class="button">Принять</button>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<style scoped>
.button {
  padding: var(--gap);
  border: none;
  border-radius: var(--border-radius);
  background-color: var(--secondary-color);
  color: var(--light-color);
  font-weight: 600;
  cursor: pointer;
}

.button:disabled {
  background-color: var(--grey-color);
}

.modal {
  position: fixed;
  top: 0px;
  right: 0px;
  bottom: 0px;
  left: 0px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgb(0 0 0 / 0.3);
  z-index: 10;
  transition: opacity 0.2s linear;
}

.modal-container {
  padding: var(--gap);
  border-radius: var(--border-radius);
  background-color: var(--light-color);
}

.modal-sub-container {
  display: flex;
  justify-content: space-between;
  gap: var(--gap);
  align-items: center;
  margin-bottom: var(--gap);
}

.modal-title {
  margin: 0;
  font-size: 16px;
  font-weight: 600;
}

.close-button {
  height: 25px;
  width: 25px;
  border: none;
  font-size: 20px;
  background-color: transparent;
  cursor: pointer;
}

.controls {
  display: flex;
  gap: var(--gap);
}
</style>
