<template>
  <li
    class="input-interface__created-todo todo-item"
    :class="{ 'input-dark item': darkTheme, 'input-light item': !darkTheme }"
    v-for="activeTodo in activeTodos"
    @click="$emit('addCompletedTodo', activeTodo.id)"
    :key="activeTodo.id"
  >
    <input
      type="checkbox"
      value="{{ activeToDo.name }}"
      class="input-interface__check"
    />
    <p :class="{ 'input-dark-font': darkTheme, 'input-light-font': !darkTheme }">
      {{ activeTodo.name }}
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
