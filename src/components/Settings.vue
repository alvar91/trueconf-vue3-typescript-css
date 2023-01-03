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
