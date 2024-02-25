<script setup>
    import { ref, onMounted } from 'vue'
    import { useTodoStore } from '@/stores/todo'

    // components
    import BaseInput from '@comp/BaseInput.vue'

    // store container
    const store = useTodoStore()

    // initial input / default input
    const defaultInput = {
        title: '',
        description: '',
        category: '',
        completed: false
    }

    // ref input
    // spread syntax
    const input = ref({ ...defaultInput })

    const editing = ref(false)

    // function reset form
    const resetForm = () => {
        Object.assign(input.value, defaultInput)

        editing.value = false
    }

    // function yang menerima submit form
    async function onSubmit(){
        // event.preventDefault();
        const data = { ...input.value }

        if(editing.value === false){
            // add list via store
            await store.addTodo({ ...input.value })
        } else {
            // edit list
            await store.editTodo(editing.value, data)
        }


        // reset form
        resetForm()
    }

    function detailTodo(id){
        const detail = store.getDetail(id)

        input.value = { ...detail.value }

        editing.value = id
    }

    async function toggleComplete(id){
        const detail = store.getDetail(id)

        await store.editTodo(id, {
            // pass all entries in detail object 
            ...detail.value,
            // take completed value then toogle it
            completed: !detail.value.completed
        })
    }

    onMounted(async () => await store.init())
</script>

<template>
    <div class="container">
        <h1>Test {{ $route.params?.id }}</h1>
    
        <!-- add v-model to integrate data binding with ref -->
        <!-- add event handler listener when keyup enter -->
        <!-- method handler with addList function -->
        <!-- event modifier .enter, .prevent -->
        <form class="form" @submit.prevent="onSubmit" @reset="resetForm">
            <BaseInput type="text" v-model="input.title" name="title" placeholder="Gaming" required/>
            <BaseInput type="text" v-model="input.description" name="description" placeholder="Everyday" required/>
            <BaseInput type="text" v-model="input.category" name="category" placeholder="Todo" />
            <div class="checkbox">
                <input type="checkbox" v-model="input.completed" name="completed"/> Completed
            </div>
            <button type="reset">Cancel</button>
            <button type="submit">{{ editing !== false ? 'Save' : 'Submit' }}</button>
        </form>

        <ol>
            <!-- (item, index) -->

            <template v-for="(item, index) in store.getTodo" v-bind:key="index">
                <!-- null chaining (?.), nullish coalescing (??); ternary operator; not operator -->
                <li :class="{ strike: item?.completed }" @dblclick="toggleComplete(item.id)">
                    <button class="red" @click="() => store.removeTodo(item.id)" :disabled="editing !== false">&times;</button>
                    <button class="orange" @click="() => detailTodo(item.id)" :disabled="editing !== false">&#9998;</button>
                    {{ item?.title }} - {{ !!item?.description ? item.description : '' }}
                </li>
            </template>
        </ol>
    </div>
</template>

<!-- style default bersifat global -->
<!-- scoped untuk melimitasi hanya di komponen -->
<!-- tambahkan lang="scss" agar bisa menggunakan fitur2 scss -->
<!-- pastikan sudah install package 'sass'; 'npm i sass' -->
<style scoped>
    /* body = font-size: 16px (1rem) */
    .form {
        margin-block-end: 2rem;
    }
    .list {
        /* rem, em, vh, vw */
        padding-block: 1rem;
        & > .strike {
            text-decoration: line-through;
        }
    }

    .checkbox {
        width: 100%;
    }

    button {
        .red {
            color: red;
        }
        .orange {
            color: orange;
        }
    }
</style>