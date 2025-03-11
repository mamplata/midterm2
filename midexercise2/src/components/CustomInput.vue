<template>
    <div class="mb-3">
        <label :for="id" class="form-label">{{ label }}</label>
        <input :type="type" :id="id" :value="modelValue" @input="$emit('update:modelValue', $event.target.value)"
            :class="['form-control', { 'is-invalid': error }]" />
        <div v-if="error" class="invalid-feedback">{{ error }}</div>
    </div>
</template>
<script setup>
defineProps(['id', 'label', 'type', 'modelValue', 'error']);
defineEmits(['update:modelValue']);
</script>