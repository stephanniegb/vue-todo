<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        what's up, <input type="text" placeholder="Name here" v-model="name">
      </h2>
    </section>
    <section class="create-todo">
      <h3>CREATE A TODO</h3>
      <form @submit.prevent="addTodo">
        <h4>What's on your todo list?</h4>
        <input type="text" placeholder="e.g read BCH 401" v-model="input_content">
        <h4>PICK A CATEGORY</h4>
        <div class="options">
          <label>
            <input type="radio" name="category" id="category1" value="business" v-model="input_category">
            <span class="bubble business"></span>
            <div>Business</div>
          </label>
          <label>
            <input type="radio" name="category" id="category2" value="personal" v-model="input_category">
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>
        <input type="submit" value="Add todo">
      </form>
    </section>
  </main>
 <section class="todo-list">
  <h3>TODO LIST</h3>
  <div class="list">
    <div v-for="todo in todo_asc" :key="todo.createdAt" :class="`todo-item ${todo.done &&  'done'}`">
      <label>
        <input type="checkbox" v-model="todo.done"/>
        <span :class="`bubble ${todo.category}`"></span>
      </label>
      <div class="todo-content">
        <input type="text" v-model="todo.content">
      </div>
      <div class="actions">
        <button class="delete" @click="removeTodo(todo)">Remove</button>
      </div>
    </div>
  </div>
 </section>
</template>


<script setup>
import {computed, onMounted, ref , watch} from 'vue'
const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)
const addTodo = () => {
  if (input_content.value.trim() === '' || input_category.value === null){
    return
  }
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime()
  })
  input_content.value = ''
  input_category.value = null
}
const removeTodo = (todo) => {
  todos.value = todos.value.filter(t => t != todo)
}
watch(todos, newTodo => {
  localStorage.setItem('todos', JSON.stringify(newTodo))

}, {deep: true})

 

const todo_asc = computed (() => {
  return todos.value.slice(0).sort((a, b) => {
    return b.createdAt - a.createdAt
  })
})


watch(name, (newVal) => {
  localStorage.setItem('name', newVal)

})
onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos'))
})
// we displayed the local storage from an onMounted because you want to display when the page mounts
</script>


