<template>
  <div class="container">
    <div class="row">
      <!-- Left Column: Registration Form -->
      <div class="col-md-6">
        <Register @userRegistered="fetchUsers" />
      </div>

      <!-- Right Column: User List -->
      <div class="col-md-6">
        <UserList v-if="submitted" :users="users" />
        <div v-else class="card p-4 shadow-lg mt-5">
          <div class="card-body text-center">
            <h1 class="text-success">User will be displayed here</h1>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Register from './views/Register.vue';
import UserList from './views/UserList.vue';
import axios from 'axios';

const users = ref([]);
const submitted = ref(false);

const fetchUsers = async () => {
  try {
    submitted.value = false;
    const response = await axios.get('https://jsonplaceholder.typicode.com/users');
    users.value = response.data;
    submitted.value = true;
  } catch (error) {
    console.error('Error fetching users', error);
  }
};
</script>
