<template>
  <todo-list-new />
  <section class="container">
    <div class="row justify-content-center m-2"><todo-list-main /></div>
  </section>
</template>

<script>
  export default {
    name: 'TodoListContainer',
  }
</script>
<script setup>
  import { ref, readonly, provide } from 'vue'
  import TodoListNew from './TodoListNew.vue'
  import TodoListMain from './TodoListMain.vue'
  import { useStorage } from '../compositions/storage'

  const todos = ref([])
  const { loadTodos, saveTodos, storage_id } = useStorage()

  provide('todos', readonly(todos))
  const initTodos = (init_todos) => {
    todos.value = init_todos
  }
  const addTodo = (job, date) => {
    todos.value.push({
      id: storage_id.value++,
      job: job,
      date: date,
      completed: false,
    })
    saveTodos(todos)
  }
  const removeTodo = (id) => {
    todos.value.splice(id, 1)
    todos.value.forEach((todo, idx) => {
      todo.id = idx
    })
    saveTodos(todos)
  }
  const completeTodo = (id) => {
    todos.value[id].completed = true
    saveTodos(todos.value)
  }

  provide('addTodo', addTodo)
  provide('removeTodo', removeTodo)
  provide('completeTodo', completeTodo)

  loadTodos(initTodos)
</script>
