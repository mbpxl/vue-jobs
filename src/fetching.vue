<template>
  <div class="header">
    <h1>{{ name }}</h1>
  </div>

  <div class="status">
    <h2 v-if="status === 'Active'">User is active</h2>
    <h2 v-else>User is inactive</h2>
    <button @click="changeTitle">Change title</button>
  </div>

  <div class="task">
    <ul>
      <li v-for="(task, index) in tasks">
        <span>{{ task }}</span>
        <button @click="deleteTask(index)">x</button>
      </li>
    </ul>

    <form @submit.prevent="addTask">
      <label for="newTask">Add task</label>
      <input type="text" id="newTask" name="newTask" v-model="newTask">
      <button type="submit">Add new task</button>
    </form>
  </div>
</template>


<script setup>
import {onMounted, ref} from 'vue'

  const name = ref("Page name");
  const status = ref("Active");
  const tasks = ref(["Task-1", "Task-2", "Task-3"]);
  const newTask = ref("");

  onMounted(async () => {
    try {
      const response = await fetch("https://jsonplaceholder.typicode.com/todos");
    const data = await response.json();
    tasks.value = data.map(task => task.title);
    } catch (error) {
      console.error("Error in fetching todos");
    }
  });

  const changeTitle = () => {
    if (status.value === "Active") {
      status.value = "Inactive";
    } else {
      status.value = "Active";
    }
  }

  const addTask = () => {
    if (newTask.value.trim() !== "") {
      tasks.value.push(newTask.value);
      newTask.value = ""
    }
  }

  const deleteTask = (index) => {
    tasks.value.splice(index, 1);
  }
</script>
