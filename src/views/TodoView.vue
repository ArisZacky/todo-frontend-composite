<script setup>
import { ref, onMounted } from 'vue'
import { useTodoStore } from '@/stores/todo'

/**
 * import component with aliase defined in vite.config.js
 */
import BaseInput from '@comp/BaseInput.vue'
import BaseTable from '@comp/BaseTable.vue'

/**
 * initiate store
 */
const store = useTodoStore()

/**
 * initial state for default input value
 * use in reset form
 */
const defaultInput = {
  title: '',
  description: '',
  category: '',
  completed: false
}
const table = {
  columns: ['id', 'title', 'description', 'completed'],
  actions: [
    {
      title: 'log',
      event: 'log-event'
    },
    {
      title: 'toggle',
      event: 'toggle-event'
    }
  ]
}

/**
 * create reactive input from default input value
 * set editing state to false
 */
const input = ref({ ...defaultInput })
const editing = ref(false)

/**
 * reset form to its initial state
 * also set editing state to false
 */
const resetForm = () => {
  Object.assign(input.value, defaultInput)

  editing.value = false
}

/**
 * submit input form
 * check if editing state is false, add todo
 * when editing state contains id, edit todo by that id
 */
async function onSubmit() {
  // event.preventDefault();
  const data = { ...input.value }

  if (editing.value === false) {
    // add list via store
    await store.addTodo(data)
  } else {
    // edit list
    await store.editTodo(editing.value, data)
  }

  // reset form
  resetForm()
}

/**
 * get detail todo from store by selected todo id
 * set input value from detail todo
 * set editing state to its id
 * @param {int} id
 */
function detailTodo(id) {
  const detail = store.getDetail(id)

  input.value = { ...detail.value }

  editing.value = id
}

/**
 * get detail todo from store by selected todo id
 * take its completed value than toggle it
 * send updated value
 * @param {int} id
 */
async function toggleComplete(id) {
  const detail = store.getDetail(id)

  await store.editTodo(id, {
    // pass all entries in detail object
    ...detail.value,
    // take completed value then toggle it
    completed: !detail.value.completed
  })
}

async function handleLogEvent(row) {
  try {
    console.log(row)
  } catch (error) {
    console.error(error)
  }
}
async function handleToggleEvent(row) {
  try {
    await toggleComplete(row.id)
  } catch (error) {
    console.error(error)
  }
}

/**
 * when page is mounted / opened in browser,
 * get todo list from back end server
 */
onMounted(async () => await store.init())
</script>

<template>
  <div class="container">
    <h1>My To Do List! {{ $route.params?.id }}</h1>

    <!-- add v-model to integrate data binding with ref -->
    <!-- add event handler listener when keyup enter -->
    <!-- method handler with addList function -->
    <!-- event modifier .enter, .prevent -->
    <b-form class="form" @submit.prevent="onSubmit" @reset="resetForm">
      <BaseInput v-model="input.title" name="title" placeholder="Gaming" required />
      <BaseInput v-model="input.description" name="description" placeholder="Everyday" required />
      <BaseInput v-model="input.category" name="category" placeholder="Todo" />
      <div class="checkbox">
        <input type="checkbox" v-model="input.completed" name="completed" /> Completed
      </div>
      <button class="cancel" type="reset">Cancel</button>
      <button class="submit" type="submit">{{ editing !== false ? 'Save' : 'Submit' }}</button>
    </b-form>

    <h4>Tasks</h4>
    <ol class="list">
      <!-- (item, index) -->
      <template v-for="(item, index) in store.getTodo" :key="index">
        <!-- null chaining (?.), nullish coalescing (??); ternary operator; not operator -->
        <li :class="{ strike: item?.completed }" @dblclick="toggleComplete(item.id)">
          <button class="red" @click="() => store.removeTodo(item.id)" :disabled="editing !== false">&times;</button>
          <button class="orange" @click="() => detailTodo(item.id)" :disabled="editing !== false">&#9998;</button>
          {{ item?.title }} - {{ !!item?.description ? item.description : '' }}
        </li>
      </template>
    </ol>

    <h4>Table</h4>
    <BaseTable
      :data="store.getTodo"
      :columns="table.columns"
      :actions="table.actions"
      @log-event="handleLogEvent"
      @toggle-event="handleToggleEvent"
    />
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

.cancel{
    background-color: #ffbd03; /* Green */
    border: none;
    border-radius: 6px;
    color: white;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 12px 12px;
}

.submit{
    background-color: #04AA6D; /* Green */
    border: none;
    border-radius: 6px;
    color: white;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
}

.checkbox {
  width: 100%;
}

.red {
    color: red;
  }
  .orange {
    color: orange;
  }

</style>
