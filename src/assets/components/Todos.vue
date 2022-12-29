<template>
  <li
    class="input-interface__created-todo todo-item"
    :class="{ 'input-dark item': darkTheme, 'input-light item': !darkTheme }"
    v-for="todo in todos"
    @click="$emit('addCompletedTodo', todo.id)"
    :key="todo.id"
  >
    <input
      type="checkbox"
      value="{{ todo.name }}"
      class="input-interface__check"
      :checked="todo.completed"
    />
    <p :class="{ 'input-dark-font': darkTheme, 'input-light-font': !darkTheme }">
      {{ todo.name }}
    </p>
  </li>
</template>

<script>
export default {
  emits: ["addCompletedTodo"],
  props: ["todos", "activeTodos", "completedTodos", "darkTheme"],
};
</script>

<style lang="scss">
@import "@/assets/scss/main.scss";
</style>
