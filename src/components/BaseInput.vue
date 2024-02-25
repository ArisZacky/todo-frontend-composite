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
    <div class="container">
        <label class="label" for="name">{{ name }}</label>
        <input 
        :id="name"
        class="input"
        :type="type"
        :name="name"
        :placeholder="placeholder"
        :value="modelValue"
        :required="required"
        @input="(e) => $emit('update:modelValue', e.target.value)">
    </div>
</template>

<style scoped>
    
    .label{
        font-size: 18px;
        font-family: Verdana, Geneva, Tahoma, sans-serif;
    }
    .input {
        /* 2rem = 16px * 2 */
        padding: 12px 8px;
        margin-block-end: 1rem;
        width: 100%;
        border: 1px solid #ccc;
        border-radius: 8px;
        box-sizing: border-box;
    }
</style>