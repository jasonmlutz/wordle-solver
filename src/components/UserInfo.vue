<script setup>
import {ref} from "vue"

const name = ref("");
const displayName = ref("");

function handleSubmit() {
  displayName.value = name.value;
}

</script>

<template> 
  <div class="UserInfo">
    <p>{{ displayName }}</p><input
      v-model="name"
      placeholder="enter a name"
    ><button @click="handleSubmit">
      SUBMIT
    </button>
  </div>
</template>

<style>
.UserInfo {
  display: flex;
  flex-direction: column;
}

.UserInfo * {
  width: 100px;
  margin: 5px auto;
}

p {
  width: auto
}
</style>

