<script setup>
    // komunikasi dari parent ke children; props
    defineProps({
        name:{
            type: String,
            default: 'name'
        },
        placeholder: {
            type: String,
            default: 'Placeholder'
        },
        modelValue: {
            type: String,
            default: ''
        },
        type: {
            type: String, // value type; ex: String, Boolean, Array
            default: 'text', // default value
            validator: (value) => ['text', 'password'].includes(value) // accepted value
        },
        required: {
            type: Boolean,
            default: false
        }
    })
    // Komunikasi dari children ke parent; emit
    defineEmits(['update:modelValue'])
</script>

<template>
    <label for="name">{{ name }}</label>
    <input 
    :id="name"
    class="input"
    :type="type"
    :name="name"
    :placeholder="placeholder"
    :value="modelValue"
    :required="required"
    @input="(e) => $emit('update:modelValue', e.target.value)">
</template>

<style scoped>
    .input {
        /* 2rem = 16px * 2 */
        margin-block-end: 1 rem;
        width: 100%;
    }
</style>