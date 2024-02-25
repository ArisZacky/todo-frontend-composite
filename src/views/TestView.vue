<script setup>
    import { reactive, ref } from 'vue'
    // Add reactive with ref
    // Ref: buat bikin object khusus berisi reactive: {set, get}
    // refImp;
    // reactive buat bikin referensi langsung ke object nya
    // proxy

    const list = reactive([
        {
            name: 'First List'
        },
        {
            name: 'Second List'
        }
    ])
</script>

<template>
    <h1>Test</h1>

    <ol>
        <template v-for="item in list" v-bind:key="item">
            <li>{{ item.name }}</li>
        </template>
    </ol>
</template>