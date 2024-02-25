<script setup>
    import { ref } from 'vue'
    import { useListStore } from '@/stores/lists'

    const store = useListStore()

    // ref input
    const nameInput = ref('')
</script>

<template>
    <div class="container">
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
    </div>
</template>

<!-- style default bersifat global -->
<!-- scoped untuk melimitasi hanya di komponen -->
<style scoped lang="scss">
    /* body = font-size: 16px (1rem) */
    .input {
        /* 2rem = 16px * 2 */
        margin-block-end: 2rem;
    }
    .list {
        /* rem, em, vh, vw */
        padding-block: 1rem;
        & > .underline {
            text-decoration: underline;
        }
    }
</style>