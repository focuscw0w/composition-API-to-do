<template>
  <li
    class="input-interface__created-todo todo-item"
    :class="{ 'input-dark item': darkTheme, 'input-light item': !darkTheme }"
    v-for="todo in todos"
    :key="todo.id"
  >
    <input
      type="checkbox"
      value="{{ todo.name }}"
      @click="$emit('addCompletedTodo', todo.id)"
      class="input-interface__check"
      :class="{ 'input-dark': darkTheme, 'input-light': !darkTheme }"
      :checked="todo.completed"
    />
    <p
      :class="{ 'input-dark-font': darkTheme, 'input-light-font': !darkTheme }"
    >
      {{ todo.name }}
    </p>
    <img
      src="../assets/images/icon-cross.svg"
      alt="remove task"
      class="remove-task"
      @click="$emit('removeTask', todo.id)"
    />
  </li>
</template>

<script>
export default {
  emits: ["addCompletedTodo", "removeTask"],
  props: ["todos", "activeTodos", "completedTodos", "darkTheme", "removeIcon"],
};
</script>

<style lang="scss">
@import "@/assets/scss/main.scss";
</style>
