<template>
    <div class="container mt-5">
        <div class="card p-4 shadow-sm rounded" style="background: #f5f5dc;">
            <h2 class="text-success text-center mb-4">User List</h2>

            <!-- Loading Spinner -->
            <div v-if="loading" class="d-flex justify-content-center my-3">
                <div class="spinner-border text-success" role="status">
                    <span class="visually-hidden">Loading...</span>
                </div>
            </div>

            <!-- User List -->
            <div v-else>
                <div v-if="!users.length" class="text-muted text-center">No users found.</div>
                <transition-group name="fade" tag="ul" class="list-group">
                    <li v-for="user in users" :key="user.id" class="list-group-item shadow-sm">
                        <strong class="text-success">{{ user.name }}</strong>
                        <br>
                        <small class="text-muted">{{ user.email }}</small>
                    </li>
                </transition-group>
            </div>
        </div>
    </div>
</template>

<script setup>
import { defineProps, ref, watchEffect } from 'vue';

const props = defineProps({
    users: Array
});

const loading = ref(false);

// Watch for user updates and trigger loading animation
watchEffect(() => {
    if (props.users.length) {
        loading.value = true;
        setTimeout(() => {
            loading.value = false;
        }, 2000);
    }
});
</script>

<style>
/* Smooth fade transition */
.fade-enter-active,
.fade-leave-active {
    transition: opacity 2s ease-in-out;
}

.fade-enter,
.fade-leave-to {
    opacity: 0;
}

/* Styling for user list */
.list-group-item {
    border-radius: 10px;
    border: none;
    margin: 5px 0;
    background: #f0fff0;
    transition: transform 0.2s ease-in-out;
}

.list-group-item:hover {
    transform: scale(1.02);
    background: #e6ffe6;
}
</style>
