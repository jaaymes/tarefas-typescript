<template>
  <div class="task-grid">
    <template v-if="tasks.length">
      <Task class="task" v-for="task in tasks" :key="task.id" :task="task" @taskDeleted="$emit('taskDeleted', task)"
        @taskStateChange="$emit('taskStateChange', task)" />
    </template>
    <p v-else class="no-task">
      Quais as Tarefas de Hoje?
    </p>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

import type { ITask } from '@/interfaces/task';

import Task from './Task.vue';

export default defineComponent({
  components: {
    Task
  },
  props: {
    tasks: {
      type: Array as () => ITask[],
      required: true
    }
  }
})
</script>

<style scoped>
.task-grid {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

.task-grid .task {
  margin: 10px;
}

.no-task {
  color: #aaa;
  font-size: 1.7rem;
}
</style>