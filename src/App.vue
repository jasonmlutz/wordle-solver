<script setup>
import UserInfoVue from './components/UserInfo.vue';
import TodoList from './components/TodoList.vue';
</script>

<template>
  <div class="container mx-auto bg-gray-500 p-4 border border-white rounded-md flex flex-col items-center">
    <UserInfoVue />
    <TodoList />
  </div>
</template>