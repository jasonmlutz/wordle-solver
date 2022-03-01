<script setup>
import { ref, reactive } from 'vue';

const newTodo = ref("");
const todoInput = ref(null);
const todos = ref([]);

let id = 0;

function createTodo() {
  todos.value.push({id: id++, text: newTodo.value})
  todoInput.value.blur();
  newTodo.value = "";
}

</script>

<template>
  <div class="w-4/5 bg-white border border-black rounded-md text-black p-2 flex flex-col items-center">
    <span class="uppercase text-xl pb-2">Todo list</span>
    <div class="flex flex-row">
      <input
        ref="todoInput"
        v-model="newTodo"
        class="text-black mx-2 mb-2 p-2 border border-black rounded-md"
        placeholder="create a task"
        @keyup.enter="createTodo"
      ><button
        class="bg-white text-black mx-2 mb-2 p-2 border border-black rounded-md hover:bg-gray-200 uppercase"
        @click="createTodo"
      >
        create
      </button>
    </div>
    <ul>
      <li
        v-for="todo in todos"
        :key="todo.key"
      >
        {{ todo.text }}
      </li>
    </ul>
  </div>
</template>