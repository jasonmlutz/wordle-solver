<script setup>
import { computed, ref } from 'vue';
import alphabet from '../resources/Alphabet';

  const props = defineProps({
    letter: {
      type: String,
      default: "",
      validator(value) {
        return value.length === 0 || (value.length === 1 && alphabet.includes(value.toUpperCase(0)))
      }
    },
    id: {
      type: String,
      default: `${Date.now()}`
    },
  })

  // think of flags as the index of the flag in flags = ["absent", "present", "correct"]
  const flag = ref(0)

  function toggleFlag () {
    flag.value = (flag.value + 1) % 3
  }

  const flaggedClasses = computed(() => {
    let classes = "m-1 h-[42px] w-[42px] flex flex-row justify-center";
    switch (flag.value) {
      case 0:
       classes += " bg-gray-600"
       break;
      case 1:
        classes += " bg-yellow-500"
        break;
      case 2:
        classes += " bg-green-800"
        break;
      default:
        break;
    }
    return classes })
</script>

<template>
  <div
    :class="flaggedClasses"
    @click="toggleFlag"
  >
    <div
      class="text-3xl"
    >
      {{ letter }}
    </div>
  </div>
</template>