<template>
  <li
    class="input-interface__created-todo todo-item"
    :class="{ 'input-dark item': darkTheme, 'input-light item': !darkTheme }"
    v-for="completedTodo in completedTodos"
    @click="$emit('addCompletedTodo', completedTodo.id)"
    :key="completedTodo.id"
  >
    <input
      type="checkbox"
      value="{{ activeToDo.name }}"
      class="input-interface__check"
      :class="{ 'input-dark': darkTheme, 'input-light': !darkTheme }"
      :checked="completedTodo"
    />
    <p :class="{ 'input-dark-font': darkTheme, 'input-light-font': !darkTheme }">
      {{ completedTodo.name }}
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
