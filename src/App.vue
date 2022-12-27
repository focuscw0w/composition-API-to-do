<template class="main">
  <img
    src="./assets/images/bg-desktop-dark.jpg"
    alt="background"
    class="background-img"
  />
  <article class="input-interface">
    <div class="flex-container">
      <h1 class="input-interface__heading">Todo</h1>
      <img
        src="./assets/images/icon-sun.svg"
        alt="sun icon"
        class="theme-setting"
        @click="toggleTheme"
      />
    </div>
    <div class="input-interface__add-todo">
      <form
        class="input-interface__form todo-item"
        action="submit"
        @submit.prevent="addToDo(toDoName)"
      >
        <input type="checkbox" class="input-interface__check" />
        <input
          type="text"
          class="input-interface__main-input"
          placeholder="Create a new todo..."
          v-model="toDoName"
        />
      </form>
      <div class="input-interface__todos">
        <ul>
          <component
            :is="currentComponent"
            :todos="todos"
            :activeTodos="filteredActiveTodos"
            :completedTodos="filteredCompletedTodos"
            @addCompletedTodo="addToCompleted($event)"
          />
        </ul>
        <div v-if="todos.length" class="active-states">
          <span class="active-states__time">5 minutes left</span>
          <ul
            class="active-states__filters"
            v-for="link in todosNav"
            :key="link.id"
          >
            <li
              :class="{ active: currentNavIndex == link.id }"
              @click="toggleNavigation(link.id, link.name, link.component)"
            >
              {{ link.name }}
            </li>
          </ul>
          <!-- clear with IDs, not with property -->
          <button type="submit" @click="clearCompletedTodos()">
            Clear Completed
          </button>
        </div>
      </div>
    </div>
  </article>
  <div class="background-theme">
    <p class="app-guide">Drag and drop to reorder list</p>
  </div>
</template>

<script>
import { reactive, toRefs } from "@vue/reactivity";
import { computed } from "@vue/runtime-core";

import Todos from "@/assets/components/Todos.vue";
import activeTodos from "@/assets/components/activeTodos.vue";
import completedTodos from "@/assets/components/completedTodos.vue";

export default {
  components: { Todos, activeTodos, completedTodos },
  setup() {
    /* STATE */
    const state = reactive({
      todos: [],
      todosNav: [
        {
          id: 0,
          name: "All",
          component: "Todos",
        },
        {
          id: 1,
          name: "Active",
          component: "activeTodos",
        },
        {
          id: 2,
          name: "Completed",
          component: "completedTodos",
        },
      ],
      activeTodos: [],
      completedTodos: [],
      currentComponent: "Todos",
      toDoName: "",
      currentNavName: "All",
      currentNavIndex: 0,
      id: 0,
    });

    /* TOGGLE THEME */

    const toggleTheme = () => {
      console.log("clicked");
    };

    /* ADD TO DO  */

    const addToDo = (toDoName) => {
      state.todos.push({ id: state.id, name: toDoName });
      state.id += 1;
      state.toDoName = "";
    };

    /* REMOVE COMPLETED TO DO */

    const removeCompletedTodos = (id) => {
      console.log(id);
    };

    /* ACTIVE TO DO */

    const filteredActiveTodos = computed(() => {
      state.activeTodos = state.todos;
      return (state.activeTodos = state.todos.filter(
        (todo) => todo.completed === undefined
      ));
    });

    /* COMPLETED TO DO */

    const filteredCompletedTodos = computed(() => {
      state.completedTodos = state.todos;
      return (state.completedTodos = state.todos.filter(
        (todo) => todo.completed !== undefined
      ));
    });

    /* CLEAR COMPLETED */

    const clearCompletedTodos = () => {
      //state.todos = state.todos.filter((todo) => todo.completed === undefined);
    };
    /* ADD TO COMPLETED */

    const addToCompleted = (todoId) => {
      state.todos[todoId]["completed"] = true;
    };

    /* TOGGLE CATEGORY */

    const toggleNavigation = (id, name, component) => {
      state.currentNavIndex = id;
      state.currentNavName = name;
      state.currentComponent = component;
    };

    return {
      ...toRefs(state),
      addToDo,
      toggleTheme,
      addToCompleted,
      toggleNavigation,
      filteredActiveTodos,
      filteredCompletedTodos,
      clearCompletedTodos,
    };
  },
};
</script>

<style lang="scss">
@import "./assets/scss/main.scss";
</style>
