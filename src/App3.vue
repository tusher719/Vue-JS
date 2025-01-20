<script setup>
import { ref, onMounted } from "vue";

    const name = ref("John Doe");
    const status = ref('active');
    const tasks = ref(['Task One', 'Task Two', 'Task Three']);
    const newTask = ref('newTask');

    const toggleStatus = () => {
      if (status.value === 'active') {
        status.value = 'pending';
      } else if (status.value === 'pending') {
        status.value = 'inactive';
      } else {
        status.value = 'active';
      }
    };

    const addTask = () => {
      if (newTask.value.trim() !== '') {
        tasks.value.push(newTask.value);
        newTask.value = '';
      }
    };

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};
onMounted(async () => {
  try{
    const response = await fetch('https://jsonplaceholder.typicode.com/todos');
    const data = await response.json();
    tasks.value = data.map((task) => task.title);
  } catch (error) {
    console.log('Error fetching tasks', error);
  }
})

</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">User is Active</p>
  <p v-else-if="status === 'pending'">User is Pending</p>
  <p v-else>User is inactive</p>

  <br>
  <form @submit.prevent="addTask">
    <label for="newTask">Add Task </label>
    <input type="text" id="newTask" name="newTask" v-model="newTask">
    <button type="submit">Add Task</button>
  </form>

  <h3>Task:</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>
        {{ task }}
      </span>
      <button class="delete" @click="deleteTask(index)">x</button>
    </li>
  </ul>
  
  <br>
  <!-- <button v-on:click="toggleStatus">Change Status</button> -->
  <button @click="toggleStatus">Change Status</button>


</template>

<style>
button{
  background-color: rgb(11, 202, 129);
  color: white;
  padding: 10px;
  border-radius: 5px;
  border: none;
  cursor: pointer;
}
.delete{
  background-color: red;
}
input[type="text"]{
  padding: 7px;
  border-radius: 5px;
  border: 1px solid #ccc;
}
</style>