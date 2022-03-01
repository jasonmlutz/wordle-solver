<script setup>
import Letter from "./Letter.vue"
import alphabet from "../resources/Alphabet";
import { v4 as uuid } from 'uuid';

// TODO prop validation; custom Letter object?
//  https://vuejs.org/guide/components/props.html#prop-validation
const props = defineProps({
  rows: {
    type: Array,
    default: alphabet.slice(0,5).map((el,index) => (
      {letter: el, id: uuid(), position: index}
    ))
  }
})

</script>

<template>
  <ul
    v-for="row in props.rows"
    :key="row.id"
    class="flex flex-row justify-between w-[300px]"
  >
    <li
      v-for="letter in row.letters"
      :key="letter.id"
      class="w-[60px] h-[60px]"
    >
      <Letter
        :key="letter.id"
        :letter="letter.letter"
        :position="letter.position"
      />
    </li>
  </ul>
</template>