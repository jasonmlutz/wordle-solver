<script setup>
import {ref} from "vue"
import { v4 as uuid } from 'uuid';

import alphabet from "./resources/Alphabet"
import Letter from "./components/Letter.vue";

const letters = ref(alphabet.slice(0,6).map(el => (
  {letter: el, id: uuid()})))
let i = 0;
</script>

<template>
  <div class="container mx-auto bg-gray-500 p-4 border border-white rounded-md flex flex-col items-center">
    <ul class="flex flex-row justify-between w-[300px]">
      <li
        v-for="letter in letters"
        :key="letter.id"
        class="w-[50px] h-[50px]"
      >
        <Letter
          :key="letter.id"
          :letter="letter.letter"
        />
      </li>
    </ul>
  </div>
</template>