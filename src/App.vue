<template>
  <div class="container">
    <HeaderComponent
      @toggle-add-task="toggleAddTask"
      title="Task Manager"
      :showAddTask="showAddTask"
    />

    <div v-show="showAddTask">
      <AddTask @add-task="addTask" />
    </div>

    <TasksComponent
      @toggle-reminder="toggleReminder"
      @delete-task="deleteTask"
      :tasks="tasks"
    />

    <FooterComponent />
  </div>
  
</template>

<script>
import HeaderComponent from "./components/HeaderComponent.vue";
import TasksComponent from "./components/TasksComponent.vue";
import AddTask from "./components/AddTask.vue";
import FooterComponent from "./components/FooterComponent.vue";

export default {
  name: "App",
  components: {
    HeaderComponent,
    TasksComponent,
    AddTask,
    FooterComponent,
  },

  data() {
    return {
      tasks: [],
      showAddTask: false,
    };
  },

  methods: {
    async addTask(task) {

      const res = await fetch('http://localhost:3000/tasks', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(task)
      });

      const data = await res.json();
      this.tasks = [...this.tasks, data];
    },

    async deleteTask(id) {
      


      if (confirm("Are you sure? This action cannot be reversed...")) {

        const res = await fetch(`http://localhost:3000/tasks/${id}`, {
          method: 'DELETE',
          headers: {
            'Content-Type': 'application/json'
          }
        });

        res.status === 200 ? this.tasks = this.tasks.filter(task => task.id !== id) : alert("Something went wrong");


      }
    },

    async toggleReminder(id) {

      const taskToToggle = await this.fetchTask(id);

      const updateTask = {
        ...taskToToggle,
        reminder: !taskToToggle.reminder
      };

      const res = await fetch(`http://localhost:3000/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(updateTask)
      });

      const data = await res.json();

      
      this.tasks = this.tasks.map((task) => {
        if (task.id === id) {
          task.reminder = data.reminder;
        }
        return task;
      });
    },

    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },

    async fetchTasks() {
      const res = await fetch("http://localhost:3000/tasks");

      const data = await res.json();

      console.log(data, "fetching from db.json")

      return data;
    },

    async fetchTask(id) {
      const res = await fetch(`http://localhost:3000/tasks/${id}`);

      const data = await res.json();

      console.log(data, "fetching from db.json")

      return data;
    },
  },

  async created() {
    this.tasks = await this.fetchTasks();

    console.log(this.tasks, "data collected from db.json")
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: "Poppins", sans-serif;
}

.container {
  max-width: 50rem;
  margin: 3rem auto;
  overflow: auto;
  min-height: 30rem;
  border: 0.1rem solid steelblue;
  padding: 3rem;
  border-radius: 0.5rem;
}

.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
