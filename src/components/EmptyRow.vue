<script setup>
import {ref} from "vue"
import Letter from "./Letter.vue"

let i = 0;
const emptyLetters = ref([
  {key: i++, letter: " "},
  {key: i++, letter: " "},
  {key: i++, letter: " "},
  {key: i++, letter: " "},
  {key: i++, letter: " "},
  {key: i++, letter: " "}
])

</script>

<template>
  <ul class="flex flex-row justify-between w-[300px]">
    <li
      v-for="letter in emptyLetters"
      :key="letter.id"
      class="w-[50px] h-[50px]"
    >
      <Letter
        :letter="letter.letter"
      />
    </li>
  </ul>
</template>