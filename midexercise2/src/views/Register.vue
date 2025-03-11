<template>
    <div class="container mt-5">
        <div class="card p-4 shadow-sm rounded" style="background: #f5f5dc;">
            <h2 class="text-success text-center mb-4">User Registration</h2>
            <form @submit.prevent="registerUser">
                <CustomInput id="name" label="Name" type="text" v-model="name" :error="errors.name" />
                <CustomInput id="email" label="Email" type="email" v-model="email" :error="errors.email" />
                <CustomInput id="password" label="Password" type="password" v-model="password"
                    :error="errors.password" />
                <CustomInput id="age" label="Age" type="number" v-model="age" :error="errors.age" />
                <button type="submit" class="btn btn-success w-100 mt-3">Register</button>
            </form>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';
import CustomInput from '../components/CustomInput.vue';

const name = ref('');
const email = ref('');
const password = ref('');
const age = ref('');
const errors = ref({});
const emit = defineEmits(['userRegistered']);

const validateForm = () => {
    errors.value = {}; // Reset errors

    // Name validation
    if (!name.value.trim()) {
        errors.value.name = 'Name is required';
    } else if (name.value.length < 3) {
        errors.value.name = 'Name must be at least 3 characters long';
    }

    // Email validation
    if (!email.value.trim()) {
        errors.value.email = 'Email is required';
    } else if (!/^[\w.-]+@[a-zA-Z\d.-]+\.[a-zA-Z]{2,}$/.test(email.value)) {
        errors.value.email = 'Invalid email format';
    }

    // Password validation
    if (!password.value.trim()) {
        errors.value.password = 'Password is required';
    } else if (password.value.length < 6) {
        errors.value.password = 'Password must be at least 6 characters';
    }

    // Age validation
    if (!age.value) {
        errors.value.age = 'Age is required';
    } else if (!/^\d+$/.test(age.value)) {
        errors.value.age = 'Age must be a valid number';
    } else if (Number(age.value) <= 18) {
        errors.value.age = 'Age must be greater than 18';
    }

    return Object.keys(errors.value).length === 0; // Return true if no errors
};

const registerUser = async () => {
    if (!validateForm()) return;
    try {
        await axios.post('https://jsonplaceholder.typicode.com/users', {
            name: name.value,
            email: email.value,
            password: password.value,
            age: age.value
        });
        emit('userRegistered');

        // Reset form fields after successful submission
        name.value = '';
        email.value = '';
        password.value = '';
        age.value = '';
        errors.value = {}; // Clear any previous validation errors
    } catch (error) {
        console.error('Registration failed', error);
    }
};
</script>

<style>
/* Make the form container softer */
.card {
    max-width: 450px;
    margin: auto;
    border-radius: 12px;
    border: none;
}

/* Error Styling */
input.is-invalid {
    border: 2px solid red;
}

/* Smooth Button Animation */
.btn-success {
    background-color: #4caf50;
    border: none;
    transition: 0.3s;
}

.btn-success:hover {
    background-color: #45a049;
    transform: scale(1.02);
}
</style>
