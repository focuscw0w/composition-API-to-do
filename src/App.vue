<template class="main">
  <img
    :src="require(`@/assets/images/${backgroundImage}`)"
    alt="background"
    class="background-img"
  />
  <article class="input-interface">
    <div class="flex-container">
      <h1 class="input-interface__heading">Todo</h1>
      <img
        :src="require(`@/assets/images/${currentThemeIcon}`)"
        alt="sun icon"
        class="theme-setting"
        @click="toggleTheme(currentThemeIcon, backgroundImage)"
      />
    </div>
    <div class="input-interface__add-todo">
      <form
        class="input-interface__form todo-item"
        :class="{ 'input-dark': darkTheme, 'input-light': !darkTheme }"
        action="submit"
        @submit.prevent="addToDo(toDoName)"
      >
        <input
          type="checkbox"
          class="input-interface__check"
          :class="{ 'input-dark': darkTheme, 'input-light': !darkTheme }"
        />
        <input
          type="text"
          class="input-interface__main-input"
          :class="{ 'bg-dark-font': darkTheme, 'bg-light-font': !darkTheme }"
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
            :darkTheme="darkTheme"
            @addCompletedTodo="toggleCompleted($event)"
          />
        </ul>
        <div
          v-if="todos.length"
          class="active-states"
          :class="{ 'input-dark': darkTheme, 'input-light': !darkTheme }"
        >
          <span class="active-states__time"
            >{{ filteredActiveTodos.length }} items left</span
          >
          <ul
            class="active-states__filters"
            :class="{ 'input-dark': darkTheme, 'input-light': !darkTheme }"
          >
            <li
              v-for="link in todosNav"
              :key="link.id"
              :class="{ active: currentNavIndex == link.id }"
              @click="toggleNavigation(link.id, link.name, link.component)"
            >
              <a>{{ link.name }}</a>
            </li>
          </ul>
          <button type="submit" @click="clearCompletedTodos()">
            Clear Completed
          </button>
        </div>
      </div>
    </div>
  </article>
  <div
    class="background-theme"
    :class="{ 'bg-dark': darkTheme, 'bg-light': !darkTheme }"
  >
    <p
      class="app-guide"
      :class="{ 'app-guide-dark': darkTheme, 'app-guide-light': !darkTheme }"
    >
      Drag and drop to reorder list
    </p>
  </div>
</template>

<script>
import { reactive, ref, toRefs } from "@vue/reactivity";
import { computed } from "@vue/runtime-core";

import Todos from "@/components/Todos.vue";
import activeTodos from "@/components/activeTodos.vue";
import completedTodos from "@/components/completedTodos.vue";

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
      backgroundImage: "bg-desktop-dark.jpg",
      toDoName: "",
      currentThemeIcon: "icon-sun.svg",
      currentComponent: "Todos",
      currentNavName: "All",
      darkTheme: true,
      currentNavIndex: 0,
      id: 0,
    });

    /* TOGGLE THEME */

    const toggleTheme = (icon, background) => {
      state.currentThemeIcon =
        icon == "icon-moon.svg" ? "icon-sun.svg" : "icon-moon.svg";

      state.backgroundImage =
        background == "bg-desktop-dark.jpg"
          ? "bg-desktop-light.jpg"
          : "bg-desktop-dark.jpg";

      state.darkTheme = !state.darkTheme
    };

    /* ADD TO DO  */

    const addToDo = (task) => {
      if (task == "") return;

      state.todos.push({ id: state.id, name: task, completed: false });
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
        (todo) => todo.completed === false
      ));
    });

    /* COMPLETED TO DO */

    const filteredCompletedTodos = computed(() => {
      state.completedTodos = state.todos;
      return (state.completedTodos = state.todos.filter(
        (todo) => todo.completed === true
      ));
    });

    /* CLEAR COMPLETED */

    const clearCompletedTodos = () => {
      state.todos = state.todos.filter((todo) => todo.completed === false);
    };

    /* ADD TO COMPLETED */

    const toggleCompleted = (todoId) => {
      state.todos[todoId].completed = !state.todos[todoId].completed;
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
      toggleCompleted,
      toggleNavigation,
      filteredActiveTodos,
      filteredCompletedTodos,
      clearCompletedTodos,
    };
  },
};
</script>

<style lang="scss">
@import "@/assets/scss/main.scss";
</style>
