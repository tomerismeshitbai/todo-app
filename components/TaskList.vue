<template>
  <div class="max-w-2xl mx-auto p-8 space-y-8 bg-gradient-to-br from-gray-100 to-gray-300 dark:from-gray-800 dark:to-gray-900 rounded-lg shadow-2xl">
    <form @submit.prevent="addTask" class="flex flex-col md:flex-row gap-4 items-center">
      <div class="relative w-full">
        <input
          v-model="newTask"
          type="text"
          placeholder="Введите задачу"
          class="border border-gray-300 dark:border-gray-700 bg-white dark:bg-gray-800 dark:text-gray-100 rounded-lg p-4 flex-1 shadow-md focus:outline-none focus:ring-2 focus:ring-blue-500 dark:focus:ring-blue-400 transition duration-300"
          :class="{'border-red-500 dark:border-red-400': showTaskError && taskError}"
        />
        <span v-if="showTaskError && taskError" class="absolute text-red-500 text-sm -bottom-6 left-0">
          {{ taskError }}
        </span>
      </div>
      <button
        type="submit"
        class="bg-blue-600 text-white rounded-lg px-6 py-3 mt-2 md:mt-0 shadow-lg hover:bg-blue-700 transition duration-300 transform hover:scale-105"
      >
        <span class="flex items-center justify-center">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4" />
          </svg>
          Добавить
        </span>
      </button>
    </form>

    <ul class="mt-6 space-y-4">
      <li
        v-for="(task, index) in tasks"
        :key="index"
        class="flex items-center justify-between py-4 px-6 border border-gray-200 dark:border-gray-700 bg-white dark:bg-gray-900 rounded-lg shadow-md transition duration-300 transform hover:scale-105 hover:bg-gray-50 dark:hover:bg-gray-800"
      >
        <template v-if="isEditing === index">
          <input
            v-model="editedTask"
            type="text"
            class="flex-1 bg-transparent border border-gray-300 dark:border-gray-600 rounded-lg p-2 text-gray-900 dark:text-gray-100 shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 dark:focus:ring-blue-400 transition duration-300"
            @keyup.enter="saveTask(index)"
            @blur="saveTask(index)"
          />
          <button
            @click="cancelEdit"
            class="ml-2 bg-gray-200 dark:bg-gray-700 text-gray-800 dark:text-gray-100 rounded-lg px-3 py-2 shadow-md hover:bg-gray-300 dark:hover:bg-gray-600 transition duration-300"
          >
            Cancel
          </button>
        </template>
        <template v-else>
          <span class="flex-1 text-gray-800 dark:text-gray-100">{{ task.name }}</span>
          <div class="flex space-x-3">
            <button
              @click="editTask(index, task.name)"
              class="bg-yellow-500 text-white rounded-lg px-3 py-2 shadow-md hover:bg-yellow-600 transition duration-300 transform hover:scale-110"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 17.5V21H4v-3.5l5.5-5.5m3-3L17 4.5l4.5 4.5L16.5 13M16.5 13L11 17.5m5.5-5.5l4.5 4.5" />
              </svg>
            </button>
            <button
              @click="deleteTask(index)"
              class="bg-red-500 text-white rounded-lg px-3 py-2 shadow-md hover:bg-red-600 transition duration-300 transform hover:scale-110"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
              </svg>
            </button>
          </div>
        </template>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: [],
      showTaskError: false, 
      isEditing: null,
      editedTask: '',
    };
  },
  computed: {
    taskError() {
      if (!this.newTask.trim()) return 'Название задачи не может быть пустым';
      if (this.newTask.trim().length < 3) return 'Название задачи должно содержать не менее 3 символов';
      return '';
    },
  },
  methods: {
    addTask() {
      this.showTaskError = true;
      if (!this.taskError) {
        this.tasks.push({ name: this.newTask.trim() });
        this.newTask = '';
        this.showTaskError = false; 
        this.saveTasks();
      }
    },
    editTask(index, taskName) {
      this.isEditing = index;
      this.editedTask = taskName;
    },
    saveTask(index) {
      if (this.editedTask.trim()) {
        this.tasks[index].name = this.editedTask.trim();
        this.isEditing = null;
        this.editedTask = '';
        this.saveTasks();
      }
    },
    cancelEdit() {
      this.isEditing = null;
      this.editedTask = '';
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
      this.saveTasks();
    },
    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
    loadTasks() {
      const tasks = localStorage.getItem('tasks');
      if (tasks) {
        this.tasks = JSON.parse(tasks);
      }
    },
  },
  mounted() {
    this.loadTasks();
  },
};
</script>

