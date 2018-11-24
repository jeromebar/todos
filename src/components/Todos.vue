<template>
  <section class="todoapp">
    <header class="header">
      <h1>Todos</h1>
      <input type="text" class="new-todo" placeholder="Ajouter une tâche" v-model="newTodo" @keyup.enter="addTodo">
    </header>

    <div class="main">
      <input type="checkbox" class="toggle-all" v-model="allDone">
      <label></label>
      <ul class="todo-list">
        <li class="todo" v-for="todo in filteredTodos" :class="{completed: todo.completed, editing: todo === editing}">
          <div class="view">
            <input type="checkbox" class="toggle" v-model="todo.completed">
            <label @dblclick="editTodo(todo)">{{todo.name}}</label>
            <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
          </div>
          <input type="text" class="edit" v-model="todo.name" @keyup.enter="doneEdit" @blur="doneEdit" @keyup.esc="cancelEdit" v-focus="todo === editing">
        </li>
      </ul>
    </div>
    <footer class="footer" v-show="hasTodos">
      <span class="todo-count"><strong>{{ remaining }}</strong> Tâche(s) à faire</span>
      <ul class="filters">
        <li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a></li>
        <li><a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">A faire</a></li>
        <li><a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Faites</a></li>
      </ul>
      <button class="clear-completed" v-show="completed" @click.prevent="deleteCompleted">Supprimer les tâches finies</button>
    </footer>
  </section>
</template>

<script>
import Vue from 'vue'

export default {
  data () {
    return {
      todos: [],
      newTodo: '',
      filter: 'all',
      editing: null,
      oldTodo: ''
    }
  },
  methods: {
    addTodo () {
      this.todos.push({
        name: this.newTodo,
        completed: false
      })
      this.newTodo = ''
    },
    deleteTodo (todo) {
      this.todos = this.todos.filter(el => el !== todo)
    },
    deleteCompleted () {
      this.todos = this.todos.filter(el => !el.completed)
    },
    editTodo (todo) {
      this.editing = todo
      this.oldTodo = todo.name
    },
    doneEdit () {
      this.editing = null
    },
    cancelEdit () {
      this.editing.name = this.oldTodo
      this.doneEdit()
    }
  },
  computed: {
    allDone: {
      get () {
        return this.remaining === 0
      },
      set (value) {
        this.todos.forEach(el => {
          el.completed = value
        })
      }
    },
    remaining () { return this.todos.filter(el => !el.completed).length },
    completed () { return this.todos.filter(el => el.completed).length },
    filteredTodos () {
      if (this.filter === 'todo') {
        return this.todos.filter(el => !el.completed)
      } else if (this.filter === 'done') {
        return this.todos.filter(el => el.completed)
      } else {
        return this.todos
      }
    },
    hasTodos (){
      return this.todos.length > 0
    } 
  },
  directives: {
    focus (el, value) {
      if (value) {
        Vue.nextTick(() => {
          el.focus()
        })
      }
    }
  }
}
</script>

<style src='./todos.css'></style>
