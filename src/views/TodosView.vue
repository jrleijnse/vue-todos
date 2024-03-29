<script setup>
import { ref, watch, computed } from 'vue';
import { uid } from 'uid';
import { Icon } from '@iconify/vue';

import TodoCreator from '../components/TodoCreator.vue';
import TodoItem from '../components/TodoItem.vue';

// Basically state
const storedTodoList = localStorage.getItem('todoList');

const todoList = ref(storedTodoList ? JSON.parse(storedTodoList) : []);

const saveTodoList = () => {
  localStorage.setItem('todoList', JSON.stringify(todoList.value));
};

// Watches to todolist array for changes and then performs a script on each change, enable deep for nested objects etc
watch(
  todoList,
  () => {
    saveTodoList();
  },
  {
    deep: true
  }
);

const todoCompleted = computed(() => {
  return todoList.value.every((todo) => todo.isCompleted);
});

// Basically state
const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: null,
    isEditing: null
  });
};

const toggleTodoComplete = (todoPos) => {
  todoList.value[todoPos].isCompleted = !todoList.value[todoPos].isCompleted;
};

const toggleEditTodo = (todoPos) => {
  todoList.value = todoList.value.map((todo, index) => {
    return {
      ...todo,
      isEditing: index === todoPos ? !todo.isEditing : false
    };
  });
};

const updateTodo = (todoVal, todoPos) => {
  todoList.value[todoPos].todo = todoVal;
};

const deleteTodo = (todoId) => {
  todoList.value = todoList.value.filter((todo) => todo.id !== todoId);
};
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo" />
    <ul v-if="todoList.length > 0" class="todo-list">
      <TodoItem
        v-for="(todo, index) in todoList"
        :key="todo.id"
        :todo="todo"
        :index="index"
        @toggle-complete="toggleTodoComplete"
        @edit-todo="toggleEditTodo"
        @update-todo="updateTodo"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p v-else class="todos-msg">
      <Icon icon="noto-v1:sad-but-relieved-face" />
      <span>You have no todo's to complete! Add one!</span>
    </p>
    <p v-if="todoCompleted && todoList.length > 0" class="todos-msg">
      <Icon icon="noto-v1:party-popper" />
      <span>All tasks completed!</span>
    </p>
  </main>
</template>

<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }
  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>
