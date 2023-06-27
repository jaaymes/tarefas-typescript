<template>
  <div class="new-task">
    <input class="form-element" @keydown.enter="add" type="text" placeholder="Nova tarefa?" v-model="name" />
    <button class="form-element" @click="add">+</button>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue';

const name = ref('')
const emit = defineEmits(['taskAdded'])
const add = () => {
  emit('taskAdded', { name: name.value })
  name.value = ''
}
</script>

<style scoped>
.new-task {
  margin: 35px;
}

.form-element {
  outline: none;
  font-size: 2rem;
  border: 1px solid #ddd;
  padding: 5px 10px 8px;
  color: #fff;
}

input.form-element {
  width: 500px;
  background: #fff2;
  border-top-left-radius: 8px;
  border-bottom-left-radius: 8px;
}

button.form-element {
  border-left: none;
  background: rgba(10, 238, 21, 0.473);
  border-top-right-radius: 8px;
  border-bottom-right-radius: 8px;
  cursor: pointer;
}
</style>