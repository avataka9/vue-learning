<template>
  <div class="todos">
    <h1>Todo application</h1>
    <hr>
    <AddTodo
        @add-todo="addTodo"
    />
    <select v-model="filter" title="choose filter">
      <option value="all">All</option>
      <option value="completed">Completed</option>
      <option value="not-completed">Not-completed</option>
    </select>
    <hr>
    <Loader v-if="loading"/>
    <TodoList
        v-else-if="filteredTodos.length"
        v-bind:todos="filteredTodos"
        @remove-todo="removeTodo"
    />
    <p v-else>No todos!</p>
  </div>
</template>

<script>
  import TodoList from '@/components/TodoList'
  import AddTodo from '@/components/AddTodo'
  import Loader from '@/components/Loader'

  export default {
    name: 'app',
    data() {
      return {
        todos: [],
        loading: true,
        filter: 'all'
      }
    },
    components: {
      TodoList,
      AddTodo,
      Loader
    },
    mounted() {
      fetch('https://jsonplaceholder.typicode.com/todos?_limit=4')
        .then(response => response.json())
        .then(json => {
          setTimeout(() => {
            this.todos = json
            this.loading = false
          }, 1000)
        })
    },
//    watch: {
//      filter(value) {
//        console.log(value)
//      }
//    },
    computed: {
      // eslint-disable-next-line
      filteredTodos() {
        if (this.filter === 'all') {
          return this.todos
        } else if (this.filter === 'completed') {
          return this.todos.filter(todo => todo.complited)
        } else if (this.filter === 'not-completed') {
          return this.todos.filter(todo => !todo.complited)
        }
      }
    },
    methods: {
      removeTodo(id) {
        this.todos = this.todos.filter( todo => todo.id !== id)
      },
      addTodo(todo) {
        this.todos.push(todo)
      }
    },
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>