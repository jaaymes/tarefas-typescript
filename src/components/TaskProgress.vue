<template>
  <div class="task-progress">
    <span class="progress-value">
      {{ progress }}%
    </span>
    <div class="progress-bar" :style="{ width: progress + '%' }"></div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  props: {
    progress: {
      type: Number,
      default: 0
    }
  }
})
</script>

<style scoped>
.task-progress {
  position: relative;
  width: 80%;
  border: 1px solid #fff;
  color: #fff;
  border-radius: 8px;
  margin-bottom: 15px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.progress-bar {
  position: absolute;
  border-radius: 8px;
  height: 100%;
  background-color: rgb(41, 214, 25);
  align-self: flex-start;
}

.progress-value {
  z-index: 1;
  font-size: 1.5rem;
}
</style>