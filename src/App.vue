<template>
  <div class="app">
    <h1>ToDoList</h1>

    <div class="add-task">
      <input v-model="newTask" placeholder="Yeni bir görev girin" @keyup.enter="addTask" />
      <button @click="addTask">Ekle</button>
      <div v-show="error" class="error">{{ error }}</div>
    </div>

    <div class="task-list">
      <h2>Tamamlanmamış görevler</h2>
      <ul>
        <li v-for="(task, index) in tasks" :key="index">
          <span :class="{ done: task.done }">{{ task.title }}</span>
          <button @click="editTask(index)">Düzenle</button>
          <button @click="deleteTask(index)">Kaldır</button>
          <button @click="toggleTaskDone(index)">
            {{ task.done ? "Tamamlanmadı olarak işaretle" : "Tamamlandı olarak işaretle" }}
          </button>
        </li>
      </ul>

      <h2>Tamamlanan görevler</h2>
      <ul>
        <li v-for="(task, index) in completedTasks" :key="index">
          <span>{{ task.title }}</span>
          <button @click="deleteCompletedTask(index)">Kaldır</button>
        </li>
      </ul>
    </div>
    <div class="stats">
      <p>Tamamlandı {{ completedTasks.length }} / {{ tasks.length }} Tamamlanmadı</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      newTask: "",
      error: "",
      tasks: [],
      editingIndex: null,
    };
  },
  methods: {
    addTask() {
      if (!this.newTask) {
        this.error = "Bu alan boş olamaz";
        return;
      }

      if (this.editingIndex === null) {
        this.tasks.push({
          title: this.newTask,
          done: false,
        });
      } else {
        this.tasks.splice(this.editingIndex, 1, {
          title: this.newTask,
          done: this.tasks[this.editingIndex].done,
        });
        this.editingIndex = null;
      }

      this.newTask = "";
      this.error = "";

      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    editTask(index) {
      this.newTask = this.tasks[index].title;
      this.editingIndex = index;
      
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);

      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    toggleTaskDone(index) {
      this.tasks[index].done = !this.tasks[index].done;

      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    deleteCompletedTask(index) {
      this.tasks.splice(index, 1);

      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    }, 
  }, 
  computed: {
    completedTasks() {
      return this.tasks.filter((task) => task.done);
    },
  },
  mounted() {
    const storedTasks = localStorage.getItem("tasks");
    if (storedTasks) {
      this.tasks = JSON.parse(storedTasks);
    }
  },
  };
</script>

<style scoped>

</style>
