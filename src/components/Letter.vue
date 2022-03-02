<script setup>
import { computed, ref, inject, watch, onMounted} from 'vue';
import alphabet from '../resources/Alphabet';

  const { updateStore } = inject("store");

  const props = defineProps({
    letter: {
      type: String,
      default: " ",
      validator(value) {
        return ((value === " ") || alphabet.includes(value.toUpperCase(0)))
      }
    },
    id: {
      type: String,
      default: `${Date.now()}`
    },
    position: {
      type: Number,
      default: undefined,
      validator(value) {
        return [0, 1, 2, 3, 4].includes(value)
      }
    },
    index: {
      type: Number,
      default: 0,
      validator(value) {
        return Array.from(Array(30)).map((_, i) => i).includes(value)
      }
    }
  })

  // think of flags as the index of the flag in flags = ["absent", "present", "correct"]
  const flag = ref(0)

  function toggleFlag () {
    flag.value = (flag.value + 1) % 3
  }

  watch(flag, () => {
    updateStore(props.letter, props.position, flag.value, props.index)
  })

  onMounted(() => {
    updateStore(props.letter, props.position, flag.value, props.index)
  })

  const flaggedClasses = computed(() => {
    let classes = "m-1 h-[52px] w-[52px] flex flex-row justify-center"
    if (props.letter === " ") {
      classes += " bg-black border-2 border-gray-600";
      return classes
    } else {
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
    return classes
    }})

</script>

<template>
  <div
    :class="flaggedClasses"
    @click="toggleFlag"
  >
    <div
      class="text-3xl pt-1.5 uppercase"
    >
      {{ letter }}
    </div>
  </div>
</template>