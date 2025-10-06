<template>
  <div class="container">
    <h1>Gestor de Tareas</h1>

    <input v-model="newTask" placeholder="Escribe el nombre de la tarea" @keyup.enter="addTask" />
    <button @click="addTask">Agregar</button>

    <div v-if="tasks.length > 0" class="board">

      <div class="column todo">
        <h2>To do</h2>
        <div
          class="task"
          v-for="(task, index) in tasks.filter(t => t.status === 'todo')"
          :key="index"
        >
          {{ task.name }}
          <button @click="moveTask(index)">→</button>
        </div>
      </div>

      <div class="column doing">
        <h2>Doing</h2>
        <div
          class="task"
          v-for="(task, index) in tasks.filter(t => t.status === 'doing')"
          :key="index"
        >
          {{ task.name }}
          <button @click="moveTask(index)">→</button>
        </div>
      </div>

      <div class="column done">
        <h2>Done</h2>
        <div
          class="task"
          v-for="(task, index) in tasks.filter(t => t.status === 'done')"
          :key="index"
        >
          {{ task.name }}
        </div>
      </div>

    </div>

    <div v-else>
      <p>No hay tareas registradas.</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const newTask = ref('')
const tasks = ref([])

const addTask = () => {
  const name = newTask.value.trim()
  if (name !== '') {
    tasks.value.push({ name, status: 'todo' })
    newTask.value = ''
  }
}

const moveTask = (index) => {
  const task = tasks.value[index]
  if (task.status === 'todo') {
    task.status = 'doing'
  } else if (task.status === 'doing') {
    task.status = 'done'
  }
}
</script>

<style scoped>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background-color: #222;
}

.container {
  max-width: 800px;
  margin: 40px auto;
  text-align: center;
  color: white;
}

input {
  padding: 10px;
  width: 60%;
  font-size: 16px;
  color: black !important;
  background-color: white !important;
  border: 1px solid #ccc;
  border-radius: 6px;
}

button {
  padding: 10px 16px;
  font-size: 16px;
  background-color: #111;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}

button:hover {
  background-color: #333;
}

.board {
  display: flex;
  justify-content: space-between;
  margin-top: 30px;
  gap: 10px;
}

.column {
  flex: 1;
  padding: 15px;
  border-radius: 12px;
}

.todo {
  background-color: #dbeeff;
}
.doing {
  background-color: #fff2d5;
}
.done {
  background-color: #e3f9e5;
}

.column h2 {
  margin-bottom: 15px;
  color: white;
}

.task {
  background-color: white;
  color: black;
  padding: 10px;
  border-radius: 10px;
  margin-bottom: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
</style>
