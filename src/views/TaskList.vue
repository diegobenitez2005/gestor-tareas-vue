<template>
    <div class="mb-4">
      <h1 class="mb-4">Lista de Tareas</h1>
      <button class="btn-custom" @click="fetchTasks">Cargar Tareas <i class="bi bi-arrow-clockwise"></i></button>
  
      <!-- Mensaje de error si falla la carga de las tareas -->
      <div v-if="error" class="alert alert-danger">
        <strong>Error:</strong> No se pudieron cargar las tareas. Intenta nuevamente más tarde.
      </div>
  
      <div v-if="tasks.length > 0" class="task-list">
        <div v-for="task in tasks" :key="task.id" class="task-item">
          <div class="task-content">
            <!-- Mostramos el título de la tarea y la marca si está completada -->
            <h5 :style="{ textDecoration: task.completed ? 'line-through' : 'none' }">{{ task.todo }}</h5>
            <span class="task-status">{{ task.completed ? 'Completada' : 'Pendiente' }}</span>
          </div>
          <div class="task-actions">
            <!-- Botón para marcar/desmarcar tarea (verde para completar) -->
            <button @click="toggleTaskCompletion(task)" class="btn-complete">
              {{ task.completed ? 'Desmarcar' : 'Completar' }}   
              <i class="bi bi-check2-all"></i>
            </button>
            <!-- Botón para eliminar tarea (rojo para eliminar) -->
            <button @click="deleteTask(task)" class="btn-delete">Eliminar <i class="bi bi-trash"></i></button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "TaskList",
    data() {
      return {
        tasks: [], // Almacenamiento local de las tareas traídas de la API
        error: false // Flag para mostrar mensaje de error en caso de fallo
      };
    },
    methods: {
      async fetchTasks() {
        this.error = false;
        try {
          const response = await fetch("https://dummyjson.com/todos");
          if (!response.ok) throw new Error('Error en la solicitud');
          const data = await response.json();
          this.tasks = data.todos;
        } catch (error) {
          console.error("Error al cargar tareas:", error);
          this.error = true;
        }
      },
  
      toggleTaskCompletion(task) {
        task.completed = !task.completed;
      },
  
      deleteTask(task) {
        this.tasks = this.tasks.filter((t) => t.id !== task.id);
      }
    },
    mounted() {
      this.fetchTasks();
    }
  };
  </script>
  
  <style scoped>
  /* Botón genérico */
  .btn-custom {
    background-color: #6c63ff;
    color: white;
    border: 2px solid #4a47a3;
    border-radius: 8px;
    padding: 10px 20px;
    font-weight: bold;
    transition: background-color 0.3s ease;
  }
  .btn-custom:hover {
    background-color: #4a47a3;
    color: #f8f9fa;
  }
  
  /* Estilo de alerta de error */
  .alert {
    padding: 10px;
    margin-top: 20px;
    background-color: #f8d7da;
    color: #721c24;
    border: 1px solid #f5c6cb;
    border-radius: 5px;
  }
  
  /* Estilos para la lista de tareas */
  .task-list {
    margin-top: 20px;
  }
  
  /* Estilo para cada tarea */
  .task-item {
    background-color: #f9f9f9; /* Fondo sutil */
    border-radius: 8px; /* Borde redondeado */
    padding: 15px;
    margin-bottom: 15px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Sombra suave */
    display: flex;
    justify-content: space-between;
    align-items: center;
    transition: all 0.3s ease;
  }
  
  .task-item:hover {
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2); /* Sombra más grande al pasar el mouse */
    transform: translateY(-5px); /* Efecto de elevación */
  }
  
  /* Estilo para el contenido de la tarea */
  .task-content {
    flex-grow: 1;
  }
  
  /* Estilo para el estado de la tarea */
  .task-status {
    font-size: 14px;
    color: #888;
    margin-left: 10px;
    font-style: italic;
  }
  
  /* Estilos para las acciones (botones) */
  .task-actions button {
    margin-left: 10px;
    padding: 8px 16px;
    border-radius: 5px;
    font-size: 14px;
    cursor: pointer;
  }
  
  /* Estilo para el botón de completar (verde) */
  .btn-complete {
    background-color: #28a745; /* Verde */
    color: white;
    border: 2px solid #218838; /* Borde verde oscuro */
  }
  .btn-complete:hover {
    background-color: #218838; /* Verde más oscuro al pasar el mouse */
  }
  
  /* Estilo para el botón de eliminar (rojo) */
  .btn-delete {
    background-color: #dc3545; /* Rojo */
    color: white;
    border: 2px solid #c82333; /* Borde rojo oscuro */
  }
  .btn-delete:hover {
    background-color: #c82333; /* Rojo más oscuro al pasar el mouse */
  }
  </style>
  