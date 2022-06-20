<template>
  <div class="todoapp">
    <section class="header">
      <h1>TODO</h1>
      <input class="new-todo" autofocus placeholder="What needs to be done?" @keyup.enter="addTodo" v-model="newTodo">
    </section>
    <section class="main">
      <input id="toggle-all" class="toggle-all" type="checkbox" v-model="allDone">
      <label for="toggle-all">Mark all as complete</label>
      <ul class="todo-list">
        <li class="todo" v-for="todo in filteredTodos" :key="todo.id" :class="{'completed': todo.done}">
          <div class="view">
            <input class="toggle" type="checkbox" v-model="todo.done">
            <label>{{todo.title}}</label>
            <button class="destroy" @click.prevent="removeTodo(todo)"></button>
          </div>
        </li>
      </ul>
    </section>
    <footer class="footer" v-show="todos.length">
      <span class="todo-count">
        <strong>Remaining: </strong>{{ remaining }}
      </span>
      <ul class="filters">
        <li><a href="#/all" :class="{'selected': filter === 'all'}" @click.prevent="filter = 'all'">All</a></li>
        <li><a href="#/active" :class="{'selected': filter === 'active'}" @click.prevent="filter = 'active'">Active</a></li>
        <li><a href="#/completed" :class="{'selected': filter === 'done'}" @click.prevent="filter = 'done'">Completed</a></li>
      </ul>
      <button class="clear-completed" @click.prevent="removeCompleted">
        Clear completed
      </button>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'MyTodo',
  data () {
    return {
      todos: [{
        id: 1,
        title: 'Une premiere tache',
        done: false
      }],
      newTodo: '',
      selectedTodo: null,
      filter: 'all'
    }
  },
  methods: {
    addTodo () {
      if (this.newTodo.length) {
        this.todos.push({
          id: this.todos.length + 1,
          title: this.newTodo,
          done: false
        })

        this.newTodo = ''
      }
    },
    removeTodo (todo) {
      this.todos = this.todos.filter(t => t.id !== todo.id)
    },
    removeCompleted () {
      this.todos = this.todos.filter(t => !t.done)
    }
  },
  computed: {
    filteredTodos () {
      if (this.filter === 'all') return this.todos
      else if (this.filter === 'active') return this.todos.filter(t => !t.done)
      else return this.todos.filter(t => t.done)
    },
    remaining () {
      return this.todos.filter(todo => !todo.done).length
    },
    allDone: {
      get () {
        return this.remaining === 0
      },
      set (value) {
        this.todos.forEach(function (t) {
          t.done = value
        })
      }
    }
  }
}
</script>

<style src="../assets/css/todo.css">

</style>
