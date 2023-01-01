<template>
  <li
    class="input-interface__created-todo todo-item"
    :class="{ 'input-dark item': darkTheme, 'input-light item': !darkTheme }"
    v-for="completedTodo in completedTodos"
    :key="completedTodo.id"
  >
    <input
      type="checkbox"
      value="{{ activeToDo.name }}"
      @click="$emit('addCompletedTodo', completedTodo.id)"
      class="input-interface__check"
      :class="{ 'input-dark': darkTheme, 'input-light': !darkTheme }"
      :checked="completedTodo"
    />
    <p
      :class="{ 'input-dark-font': darkTheme, 'input-light-font': !darkTheme }"
    >
      {{ completedTodo.name }}
    </p>
    <img
      src="../assets/images/icon-cross.svg"
      alt="remove task"
      class="remove-task"
      @click="$emit('removeTask', completedTodo.id)"
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
