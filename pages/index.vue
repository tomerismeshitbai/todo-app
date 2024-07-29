<template>
    <title>To-Do List</title>
    <div class="min-h-screen bg-gradient-to-br from-cyan-400 via-gray-300 to-cyan-600 dark:from-gray-900 dark:to-gray-800 text-gray-900 dark:text-gray-100">
      <div class="container mx-auto px-4 py-12">
        <h1 class="text-5xl font-extrabold mb-8 text-center text-gray-900 dark:text-gray-100 leading-tight tracking-widest">
          To-Do List
        </h1>
        <TaskList />
      </div>
    </div>
  </template>

  <script>
  import TaskList from '@/components/TaskList.vue';
  
  export default {
    components: {
      TaskList,
    },
  };
  </script>

<style scoped>
h1 {
    font-family:Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
}
</style>
  