<template>
  <main>
    <header>
      <img src="./assets/piana-logo.svg" alt="logo">
      <h1>{{ name }}'s tasks</h1>
    </header>

    <div class="new-task-form">
      <TaskForm />
    </div>

    <!-- filter -->
    <nav class="filter">
      <button @click.prevent="filter = 'all'">All tasks</button>
      <button @click.prevent="filter = 'favs'">Fav tasks</button>
    </nav>

    <!-- loading -->
    <div class="loading" v-if="loading">Loading tasks...</div>

    <!-- task list -->
    <div class="task-list" v-if="filter === 'all'">
      <p>All tasks: {{ totalCount }}</p>
      <div v-for="task in tasks" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>
    <div class="task-list" v-if="filter === 'favs'">
      <p>Favorite tasks: {{ favCount }}</p>
      <div v-for="task in favs" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>
    <button @click.prevent="taskStore.$reset">Reset</button>
  </main>
</template>

<script>
import { ref } from '@vue/reactivity';
import { storeToRefs } from 'pinia';
import TaskDetails from './components/TaskDetails.vue'
import TaskForm from './components/TaskForm.vue';
import { useTaskStore } from './stores/TaskStore';
export default {
  components: { TaskDetails, TaskForm },
  setup() {
    const taskStore = useTaskStore()
    const { tasks, loading, favs, totalCount, favCount } = storeToRefs(taskStore)
    // fetch task
    taskStore.getTasks()
    const filter = ref('all')

    return { taskStore, filter, tasks, loading, favs, totalCount, favCount }
  }
}
</script>
<style scoped>

</style>
