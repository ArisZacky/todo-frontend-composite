<script setup>
    import { ref } from 'vue'
    import { useListStore } from '@/stores/lists'

    // store container
    const store = useListStore()

    // initial input / default input
    const defaultInput = {
        name: '',
        hobby: ''
    }

    // ref input
    // spread syntax
    const input = ref({ ...defaultInput })

    // function yang menerima submit form
    function onSubmit(){
        // event.preventDefault();
        console.log({ ...input.value })

        // add list via store
        store.addList({ ...input.value })
    }
</script>

<template>
    <div class="container">
        <h1>Test</h1>
    
        <!-- add v-model to integrate data binding with ref -->
        <!-- add event handler listener when keyup enter -->
        <!-- method handler with addList function -->
        <!-- event modifier .enter, .prevent -->
        <form class="form" @submit.prevent="onSubmit">
            <input type="text" class="input" v-model="input.name" name="name" placeholder="John">
            <input type="text" class="input" v-model="input.hobby" name="hobby" placeholder="Gaming">
            <button type="submit">Submit</button>
        </form>

        <ol>
            <!-- (item, index) -->

            <template v-for="(item, index) in store.getList" v-bind:key="index">
                <li class="underline">{{ item.name }} ({{ item.hobby }}) - {{ index }}</li>
            </template>
        </ol>
    </div>
</template>

<!-- style default bersifat global -->
<!-- scoped untuk melimitasi hanya di komponen -->
<style scoped lang="scss">
    /* body = font-size: 16px (1rem) */
    .form {
        .input {
            /* 2rem = 16px * 2 */
            margin-block-end: 1rem;
            width: 100%;
        }

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