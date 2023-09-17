<script setup>
import { reactive, defineEmits } from 'vue';
import TodoButton from './TodoButton.vue';

const emit = defineEmits(['create-todo']);

// Basically state
const todoState = reactive({
  todo: '',
  invalid: null,
  errMsg: ''
});

const createTodo = () => {
  todoState.invalid = null;
  if (todoState.todo !== '') {
    // 'emits' a custom event called create-todo which also contains the todoState.todo value, so the parent compont will have access to this value when the custom event is triggered. The value of the event will automatically get passed into the argument of the callback function in the event handler
    emit('create-todo', todoState.todo);
    todoState.todo = '';
    return;
  }
  todoState.invalid = true;
  todoState.errMsg = 'Todo value cannot be empty';
};
</script>

<template>
  <!-- only applies class if condition after : is true -->
  <div class="input-wrap" :class="{ 'input-err': todoState.invalid }">
    <input v-model="todoState.todo" type="text" />
    <!-- Possible to make the button content dynamic due to the slot element native to vue -->
    <TodoButton @click="createTodo()" />
  </div>
  <!-- <p v-if="todoState.invalid" class="err-msg">{{ todoState.errMsg }}</p> -->
  <!-- only applies if true, v-show toggles display: none and block -->
  <p v-show="todoState.invalid" class="err-msg">{{ todoState.errMsg }}</p>
</template>

<style lang="scss" scoped>
.input-wrap {
  display: flex;
  transition: 250ms ease;
  border: 2px solid #41b080;

  &.input-err {
    border-color: red;
  }

  &:focus-within {
    box-shadow:
      0 -4px 6px -1px rgb(0 0 0 / 0.1),
      0 -2px 4px -2px rgb(0 0 0 / 0.1);
  }

  input {
    width: 100%;
    padding: 8px 6px;
    border: none;

    &:focus {
      outline: none;
    }
  }
}

.err-msg {
  margin-top: 6px;
  font-size: 12px;
  text-align: center;
  color: red;
}
</style>
