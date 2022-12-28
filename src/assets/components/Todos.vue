<template>
  <li
    class="input-interface__created-todo todo-item"
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
    <p>
      {{ todo.name }}
    </p>
  </li>
</template>

<script>
export default {
  emits: ["addCompletedTodo"],
  props: ["todos", "activeTodos", "completedTodos"],
};
</script>

<style lang="scss">
@import "@/assets/scss/main.scss";
</style>
