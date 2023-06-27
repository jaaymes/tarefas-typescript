
<template>
  <div id="app">
    <TaskProgress :progress="progress" />
    <NewTask @taskAdded="addTask" />
    <TaskGrid :tasks="tasks" @taskDeleted="taskDeleted" @taskStateChange="taskStateChange" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

import NewTask from '@/components/NewTask.vue';
import TaskGrid from '@/components/TaskGrid.vue';
import TaskProgress from '@/components/TaskProgress.vue';
import type { ITask } from '@/interfaces/task';
import api from '@/services/api';
import { v4 as uuidv4 } from "uuid";


export default defineComponent({
  name: 'App',
  components: {
    TaskProgress,
    TaskGrid,
    NewTask
  },
  data(): { tasks: ITask[] } {
    return {
      tasks: []
    }
  },
  computed: {
    //  progresso de tarefas concluidas pending === false
    progress(): number {
      const total = this.tasks.length
      const completed = this.tasks.filter(t => !t.pending).length
      return Math.round((completed / total) * 100)
    }
  },
  methods: {
    async loadData() {
      const { data } = await api.get('/tasks')
      this.tasks = data
    },
    async addTask(task: ITask) {
      const sameName = (t: ITask) => t.name === task.name
      if (!this.tasks.some(sameName)) {
        const taskData = {
          id: uuidv4(),
          name: task.name,
          pending: true
        }
        await api.post('/tasks', taskData)
        this.loadData()
        this.tasks.push(taskData)
        return
      }
      this.$toast.error('Tarefa já cadastrada!', {
        position: 'top-right',
      })
    },
    async taskDeleted(task: ITask) {
      await api.delete(`/tasks/${task.id}`)
      this.loadData()
    },
    async taskStateChange(task: ITask) {
      const taskData = {
        id: task.id,
        name: task.name,
        pending: !task.pending
      }
      await api.put(`/tasks/${task.id}`, taskData)
      this.loadData()
    }
  },
  mounted() {
    this.loadData()
  }
}) 
</script>


<style>
body {
  font-family: "Lato", sans-serif;
  background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
  color: #fff;
}

#app {
  display: flex;
  flex: 1;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

#app h1 {
  margin-bottom: 5px;
  font-weight: 300;
  font-size: 3rem;
}
</style>
