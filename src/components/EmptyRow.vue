<script setup>
import {ref} from "vue"
import Letter from "./Letter.vue"

const props = defineProps({
  copies: {
    type: Number,
    default: 1,
    validate(value) {
      return [0, 1, 2, 3, 4, 5, 6].includes(value)
    }
  }
})

let i = 0;
const emptyLetters = ref([
  {key: i++, letter: " "},
  {key: i++, letter: " "},
  {key: i++, letter: " "},
  {key: i++, letter: " "},
  {key: i++, letter: " "},
])

const rows = ref(
  Array.from(Array(props.copies)).map((e, i) => {
    return {key: i}
  }
  ))

</script>

<template>
  <ul 
    v-for="row in rows"
    :key="row.key"
    class="flex flex-row justify-between w-[300px]"
  >
    <li
      v-for="letter in emptyLetters"
      :key="letter.id"
      class="w-[60px] h-[60px]"
    >
      <Letter
        :letter="letter.letter"
      />
    </li>
  </ul>
</template>