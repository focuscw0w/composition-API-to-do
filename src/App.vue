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
        <ul v-if="currentNavName === 'All'">
          <li
            class="input-interface__created-todo todo-item"
            v-for="todo in todos"
            @click="addToCompleted(todo.id)"
            :key="todo.id"
          >
            <input
              type="checkbox"
              value="{{ todo.name }}"
              class="input-interface__check"
            />
            <p :class="{ completedTask: todos[todo.id].completed }">
              {{ todo.name }}
            </p>
          </li>
        </ul>
        <!-- there will be dynamic component 
          <activeTodos :filteredActiveTodos="filteredActiveTodos" :todos="todos"/>
        -->
        <ul v-if="currentNavName === 'Active' ">
          <li
            class="input-interface__created-todo todo-item"
            v-for="activeTodo in filteredActiveTodos"
            @click="addToCompleted(activeTodo.id)"
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
        <ul v-if="currentNavName === 'Completed'">
          <li
            class="input-interface__created-todo todo-item"
            v-for="completedTodo in filteredCompletedTodos"
            @click="addToCompleted(completedTodo.id)"
            :key="completedTodo.id"
          >
            <input
              type="checkbox"
              value="{{ activeToDo.name }}"
              class="input-interface__check"
            />
            <p :class="{ completedTask: todos[completedTodo.id].completed }">
              {{ completedTodo.name }}
            </p>
          </li>
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
              @click="toggleNavigation(link.id, link.name)"
            >
              {{ link.name }}
            </li>
          </ul>
          <button type="submit">Clear Completed</button>
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
import { onMounted } from "vue";

import activeTodos from "@/assets/components/activeTodos.vue";

export default {
  components: { activeTodos },
  setup() {
    /* STATE */
    const state = reactive({
      todos: [],
      todosNav: [
        {
          id: 0,
          name: "All",
        },
        {
          id: 1,
          name: "Active",
        },
        {
          id: 2,
          name: "Completed",
        },
      ],
      activeTodos: [],
      completedTodos: [],
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

    /* ADD TO COMPLETED */

    const addToCompleted = (todoId) => {
      state.todos[todoId]["completed"] = true;
    };

    /* COMPLETED TO DO */

    const filteredCompletedTodos = computed(() => {
      state.completedTodos = state.todos;
      return (state.completedTodos = state.todos.filter(
        (todo) => todo.completed !== undefined
      ));
    });

    /* TOGGLE CATEGORY */

    const toggleNavigation = (id, name) => {
      state.currentNavIndex = id;
      state.currentNavName = name;
    };

    return {
      ...toRefs(state),
      addToDo,
      toggleTheme,
      addToCompleted,
      toggleNavigation,
      filteredActiveTodos,
      filteredCompletedTodos,
    };
  },
};
</script>

<style lang="scss">
@import "./assets/scss/main.scss";
</style>
