<script setup>
import {ref, computed} from "vue"

const props = defineProps({
  count: {
    type: Number,
    default: 1,
    validator(value) {
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

const rows = computed(() =>
  Array.from(Array(props.count)).map((e, i) => {
    return {key: i}
  })
)

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
      <div class="m-1 h-[52px] w-[52px] flex flex-row justify-center bg-black border-2 border-gray-600">
        <div
          class="text-3xl pt-1.5 uppercase"
        >
          {{ letter.letter }}
        </div>
      </div>
    </li>
  </ul>
</template>