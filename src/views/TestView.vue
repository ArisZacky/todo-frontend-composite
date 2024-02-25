<script setup>
    import { ref } from 'vue'
    import { useListStore } from '@/stores/lists'

    const store = useListStore()

    // ref input
    const nameInput = ref('')
</script>

<template>
    <h1>Test</h1>

    <!-- add v-model to integrate data binding with ref -->
    <!-- add event handler listener when keyup enter -->
    <!-- method handler with addList function -->
    <!-- event modifier .enter -->
    <input type="text"
    v-model="nameInput"
    name="name"
    @keyup.enter="store.addList(nameInput); nameInput = ''">
    <ol>
        <template v-for="item in store.getList" v-bind:key="item">
            <li>{{ item.name }}</li>
        </template>
    </ol>
</template>