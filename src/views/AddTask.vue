<template>
  <div class="add-task-container">
    <h1 class="title">Añadir Tarea</h1>
    
    <!-- Formulario para añadir una nueva tarea -->
    <div class="input-group">
      <input
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Añadir nueva tarea"
        class="task-input"
      />
      <button @click="addTask" class="add-button">Añadir <i class="bi bi-bookmark-check-fill"></i></button>
    </div>
    
    <!-- Lista de tareas añadidas -->
    <div v-if="tasks.length > 0" class="task-list">
      <div v-for="task in tasks" :key="task.id" class="task-item">
        <span :class="{ completed: task.completed }">{{ task.todo }}</span>
        <div>
          <button @click="toggleTaskCompletion(task)" class="btn-complete">
            {{ task.completed ? 'Desmarcar' : 'Completar' }} <i class="bi bi-check2-all"></i>
          </button>
          <button @click="deleteTask(task)" class="btn-delete">Eliminar <i class="bi bi-trash"></i></button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AddTask",
  data() {
    return {
      newTask: "", // Campo de entrada para la nueva tarea
      tasks: [],   // Lista de tareas locales
    };
  },
  created() {
    // Cargar tareas desde localStorage cuando el componente se crea
    this.loadTasks();
  },
  methods: {
    addTask() {
      if (this.newTask.trim() === "") return;
      
      const newTask = {
        todo: this.newTask,
        completed: false,
        id: Date.now(),
      };
      
      // Añadir la nueva tarea al inicio de la lista
      this.tasks.unshift(newTask);
      this.newTask = "";
      
      // Guardar en localStorage después de añadir
      this.saveTasks();
    },
    
    deleteTask(task) {
      this.tasks = this.tasks.filter((t) => t.id !== task.id);
      
      // Guardar en localStorage después de eliminar
      this.saveTasks();
    },
    
    toggleTaskCompletion(task) {
      task.completed = !task.completed;
      
      // Guardar en localStorage después de cambiar estado
      this.saveTasks();
    },
    
    // Método para guardar tareas en localStorage
    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
    
    // Método para cargar tareas desde localStorage
    loadTasks() {
      const savedTasks = localStorage.getItem('tasks');
      if (savedTasks) {
        try {
          this.tasks = JSON.parse(savedTasks);
        } catch (e) {
          console.error('Error al cargar tareas:', e);
          this.tasks = [];
        }
      }
    }
  },
};
</script>

<style scoped>
.add-task-container {
  padding: 20px;
  /* max-width: 1020px; */
  margin: 0 auto;
  background-color: #f8f9fa;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  width: fit-content;
}

.title {
  text-align: center;
  margin-bottom: 20px;
  color: #4e73df;
}

.input-group {
  display: flex;
  margin-bottom: 20px;
}

.task-input {
  flex-grow: 1;
  padding: 10px;
  margin-right: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
}

.add-button {
  padding: 10px 15px;
  border: none;
  border-radius: 5px;
  background-color: #4e73df;
  color: white;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s ease;
}

.add-button:hover {
  background-color: #3e59b9;
}

.task-list {
  margin-top: 20px;
}

.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  margin-bottom: 10px;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
  width: fit-content;
}

.task-item:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
  transform: translateY(-5px);
}

.completed {
  text-decoration: line-through;
  color: #888;
}

.btn-complete, .btn-delete {
  padding: 8px 12px;
  margin-left: 10px;
  font-size: 14px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.btn-complete {
  background-color: #28a745;
  color: white;
}

.btn-complete:hover {
  background-color: #218838;
}

.btn-delete {
  background-color: #dc3545;
  color: white;
}

.btn-delete:hover {
  background-color: #c82333;
}
</style>