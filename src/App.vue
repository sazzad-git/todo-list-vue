<script setup>
import { computed, reactive, ref } from "vue";
import SingleTodoVue from "./components/SingleTodo.vue";

import SingleTodo from "./components/SingleTodo.vue";

const todo = ref("");

const todos = reactive([
  {
    id: 1,
    title: "Todo one",
    completed: false,
  },
  {
    id: 2,
    title: "Todo two",
    completed: true,
  },
  {
    id: 3,
    title: "Todo three",
    completed: false,
  },
]);

const addTodo = () => {
  if (todo.value === "") {
    return;
  }

  todos.push({
    id: todos.length + 1,
    title: todo.value,
    completed: false,
  });
  todo.value = "";
};

const countAllTodos = computed(() => {
  return todos.length;
});
const countCompleteTodos = computed(() => {
  return todos.filter((todo) => todo.completed).length;
});

const checkComplete = (index) => {
  todos[index].completed = !todos[index].completed;
};

const removeTodo = (index) => {
  todos.splice(index, 1);
};
</script>

<template>
  <div
    class="max-w-md mx-auto bg-white shadow-lg overflow-hidden mt-16 border p-6 space-y-6"
  >
    <h1 class="text-2xl uppercase text-gray-700 font-bold">
      To Do List
      <span v-if="todos.length"
        >({{ countCompleteTodos }}/{{ countAllTodos }})</span
      >
    </h1>
    <form @submit.prevent="addTodo">
      <div class="flex items-center border-b-2 border-teal-500 py-2">
        <input
          v-model="todo"
          class="border-none focus:outline-none w-full text-gray-700"
          type="text"
          placeholder="Add a task"
          name=""
          id=""
        />
        <button
          class="flex-shrink-0 bg-teal-500 hover:bg-teal-700 border-4 text-sm hover:border-teal-700 text-white py-1 px-2 rounded"
          type="submit"
        >
          Add
        </button>
      </div>
    </form>
    <ul v-if="todos.length">
      <li v-for="(todo, index) in todos" :key="todo.id">
        <SingleTodo
          :index="index"
          :todo="todo"
          @delete="removeTodo"
          @checkComplete="checkComplete"
        />
      </li>
    </ul>

    <div v-else>
      <p>No tasks yet.</p>
    </div>
  </div>
</template>

<style scoped></style>
