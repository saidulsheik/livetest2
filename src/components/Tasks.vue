
<script setup>
import { ref } from 'vue';

const tasks = ref([
  { name: 'Task 1', time: 30 },
  { name: 'Task 2', time: 40 },
  { name: 'Task 3', time: 60 },
  { name: 'Task 4', time: 45 },
  { name: 'Task 5', time: 50 }
]);

const editedTask = ref(null);
const isEditing = ref(false);

const editTask = (index) => {
  editedTask.value = { ...tasks.value[index] };
  isEditing.value = true;
  console.log("Edited Task:", editedTask.value); // Log editedTask value
};

const updateTask = () => {
  const index = tasks.value.findIndex(task => task.name === editedTask.value.name);
  console.log("Index:", index); // Log index value
  tasks.value.splice(index, 1, editedTask.value);
  isEditing.value = false;
  console.log("Tasks:", tasks.value); // Log tasks array after update
};

const closeForm = () => {
  isEditing.value = false;
};
</script>

<template>
  <div>
    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        {{ task.name }} - {{ task.time }}
        <button @click="editTask(index)">Edit</button>
      </li>
    </ul>

    <div v-if="isEditing">
      <div class="overlay" @click="closeForm"></div>
      <div class="popup">
        <h2>Edit Task</h2>
        <form @submit.prevent="updateTask">
          <label>Name:</label>
          <input type="text" v-model="editedTask.name" required>
          <label>Time:</label>
          <input type="number" v-model="editedTask.time" required>
          <button type="submit">Update</button>
        </form>
      </div>
    </div>
  </div>
</template>


<style scoped>
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
}

.popup {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: white;
  padding: 20px;
  border-radius: 5px;
}
</style>
