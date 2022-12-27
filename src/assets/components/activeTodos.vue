<template>
  <li
    class="input-interface__created-todo todo-item"
    v-for="activeTodo in activeTodos"
    @click="$emit('addCompletedTodo', activeTodo.id)"
    :key="activeTodo.id"
  >
    <input
      type="checkbox"
      value="{{ activeToDo.name }}"
      class="input-interface__check"
    />
    <p :class="{ completedTask: todos[activeTodo.id].completed }">
      {{ activeTodo.name }}
    </p>
  </li>
</template>

<script>
export default {
  emits: ["addCompletedTodo"],
  props: ["todos", "activeTodos", "completedTodos"]
};
</script>

<style lang="scss">
@import "@/assets/scss/main.scss";
</style>
