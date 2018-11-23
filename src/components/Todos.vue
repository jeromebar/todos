<template>
  <section class="todoapp">
    <header class="header">
      <h1>Todos</h1>
      <input type="text" class="new-todo" placeholder="Ajouter une tâche" v-model="newTodo" @keyup.enter="addTodo">
    </header>
    <div class="main">
      <ul class="todo-list">
        <li class="todo" v-for="todo in todos" :class="{completed: todo.completed}">
          <div class="view">
            <input type="checkbox" class="toggle" v-model="todo.completed">
            <label>{{todo.name}}</label>
          </div>
        </li>
      </ul>
    </div>
    <footer class="footer">
      <span class="todo-count"><strong>{{ remaining }}</strong> Tâches à faire</span>
    </footer>
  </section>
</template>

<script>
export default {
  data () {
    return {
      todos: [{
        name: 'Tache de test',
        completed: false
      }],
      newTodo: '',
    }
  },
  methods: {
    addTodo () {
      this.todos.push({
        name: this.newTodo,
        completed: false
      })
      this.newTodo = ''
    }
  },
  computed: {
    remaining () {
      this.remaining = this.todos.filter(el => !el.completed)
    }
  }
}
</script>

<style src="./todos.css"></style>
