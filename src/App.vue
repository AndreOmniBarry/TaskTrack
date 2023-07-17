<template>
  <div class="app">
    <h2>Tasks</h2>
    <form @submit.prevent="addTask">
      <input v-model="newTask.text" placeholder="Add a new task">
      <select v-model="newTask.priority">
        <option value="">Select priority</option>
        <option value="high">high</option>
        <option value="medium">Medium</option>
        <option value="low">Low</option>
      </select>
      <input type="date" v-model="newTask.dueDate" placeholder="Due date">
      <button type="submit">Add</button>
    </form>

    <div class="filters">
      <label>Filter by priority:</label>
      <select v-model="priorityFilter">
        <option value="">All</option>
        <option value="high">high</option>
        <option value="medium">Medium</option>
        <option value="low">Low</option>
      </select>
      <label>Search:</label>
      <input v-model="searchTerm" placeholder="Type here to search">
    </div>

    <ul>
      <li v-for="(task, index) in filteredTasks" :key="index" :class="{ completed: task.completed }">
        <div>
          <h3 class="task-title">{{ task.text }}</h3>
          <div class="details">
            <span class="priority" :class="task.priority">{{ task.priority }}</span>
            <span class="due-date">{{ task.dueDate }}</span>
          </div>
        </div>
        <div class="actions">
          <button @click="toggleCompleted(task)"> {{ task.completed ? 'Incomplete' : 'Complete' }}</button>
          <button @click="removeTask(index)">Remove</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import { ref, computed } from 'vue'

export default {
  setup() {
    const tasks = ref([
      { text: 'Buy groceries', completed: false, priority: 'high', dueDate: '2023-07-18' },
      { text: 'Do laundry', completed: false, priority: 'medium', dueDate: '2023-07-20' },
      { text: 'Walk the dog', completed: false, priority: 'low', dueDate: '2023-07-23' },
    ])
    const newTask = ref({ text: '', priority: '', dueDate: '' })
    const priorityFilter = ref('')
    const searchTerm = ref('')

    const addTask = () => {
      if (newTask.value.text) {
        tasks.value.push({
          text: newTask.value.text,
          completed: false,
          priority: newTask.value.priority,
          dueDate: newTask.value.dueDate,
        })
        newTask.value = { text: '', priority: '', dueDate: '' }
      }
    }

    const removeTask = (index) => {
      tasks.value.splice(index, 1)
    }

    const toggleCompleted = (task) => {
      task.completed = !task.completed
    }

    const filteredTasks = computed(() => {
      return tasks.value.filter(task => {
        return (priorityFilter.value ? task.priority === priorityFilter.value : true) &&
               (searchTerm.value ? task.text.toLowerCase().includes(searchTerm.value.toLowerCase()) : true)
      })
    })

    return { tasks, newTask, priorityFilter, searchTerm, addTask, removeTask, toggleCompleted, filteredTasks }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=San+Francisco&display=swap');
.app {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
 font-family: 'San Francisco', sans-serif;
}

h1 {
  margin-top: 0;
}

form {
  display: flex;
  margin-bottom: 20px;
}

input[type="date"] {
  margin-left: 10px;
}

select {
  margin-left: 10px;
}

button {
  margin-left: 10px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  padding: 5px 10px;
  cursor: pointer;
}

.filters {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

li {
  padding: 10px;
  border-bottom: 1px solid #ccc;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

li:last-child {
  border-bottom: none;
}

li.completed {
  opacity: 0.5;
}

.task-title {
  margin-top: 0;
  margin-bottom: 5px;
}

.details {
  display: flex;
  align-items: center;
}

.priority {
  display: inline-block;
  padding: 2px 5px;
  font-size: 12px;
  border-radius: 5px;
  color: #fff;
  margin-right: 5px;
}

.priority.high {
  background-color: #d83f15;
}

.priority.medium {
  background-color: #f0ad4e;
}

.priority.low {
  background-color: #5cb85c;
}

.due-date {
  font-size: 12px;
  color: #999;
}

.actions {
  display: flex;
  align-items: center;
}

.actions button {
  margin-left: 10px;
  background-color: #d83f15;
}

.actions button:hover {
  background-color: #a30000;
}

input[type="text"], select {
  padding: 5px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 10px;
}

input[type="date"] {
  padding: 5px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 10px;
  width: 170px;
}

button {
  padding: 5px 30px;
  font-size: 16px;
  background-color: #0066cc;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin: 0px 0px 0px 30px;
}

button:hover {
  background-color: #0052a3;
}

@media screen and (max-width: 600px) {
  .app {
    padding: 10px;
  }

  form {
    flex-direction: column;
  }

  input[type="date"] {
    width: 100%;
    margin-left: 0;
    margin-top: 10px;
  }

  select {
    margin-left: 0;
    margin-top: 10px;
  }

  button {
    margin: 10px 0;
  }

  .filters {
    flex-direction: column;
  }
}

@media screen and (max-width: 768px) {
  .app {
    max-width: 90%;
  }
}
</style>
