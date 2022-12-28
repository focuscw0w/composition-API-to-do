<template>
  <li
    class="input-interface__created-todo todo-item"
    v-for="completedTodo in completedTodos"
    @click="$emit('addCompletedTodo', completedTodo.id)"
    :key="completedTodo.id"
  >
    <input
      type="checkbox"
      value="{{ activeToDo.name }}"
      class="input-interface__check"
      :checked="completedTodo"
    />
    <p>
      {{ completedTodo.name }}
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
