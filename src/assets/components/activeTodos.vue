<template>
  <ul>
    <li
      class="input-interface__created-todo todo-item"
      v-for="activeTodo in filteredActiveTodos"
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
  </ul>
</template>

<script>
export default {
  props: ["filteredActiveTodos", "todos"],
  setup(props) {

  },
};
</script>

<style lang="scss">
@import "@/assets/scss/main.scss";
</style>
