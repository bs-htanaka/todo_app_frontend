<template>
  <div class="m-x-auto">
    <h2>ToDO List</h2>
    <div v-if="!isEditing">
      <input type="input" v-model='todo'>
      <v-btn @click='addTodo'>ADD</v-btn>
    </div>
    <div v-else>
      <input type="input" v-model='todo.name'>
      <v-btn @click='updateTodo'>update</v-btn>
    </div>
    <div>
      <ul>
        <v-list v-for="(todo, index) in todos" :key="index">
          <span> {{ todo.name }} </span>

          <v-btn @click="editTodo(index, todo)">edit</v-btn>
          <v-btn @click="deleteTodo(todo)">delete</v-btn>
        </v-list>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'ToDoList',
  data() {
    return {
      todo: '',
      todos: [],
      storageKey: 'vue-todos',
      isEditing: false,
      selectedIndex: null
    }
  },
  mounted(){
    axios
      .get('http://localhost:3000/todos')
      .then( response => (this.todos = response.data))
  },
  methods: {
    addTodo: function () {
      if (this.todo === '') {
        return
      }
      let todo = {
        id: 1,
        name: this.todo
      }
      axios
      .post('http://localhost:3000/todos', todo)
      .then( response => (this.todos = response.data))
      this.todo = ''
    },
    editTodo: function (index, todo) {
      this.todo = todo
      this.selectedIndex = index
      this.isEditing = true
    },
    updateTodo: function () {
      const id = this.todo.id
      axios
      .put(`http://localhost:3000/todos/${id}`, this.todo)
      .then( response => (this.todos = response.data))
      this.todo = ''
    },
    deleteTodo: function (todo) {
      const id = todo.id
      axios
      .delete(`http://localhost:3000/todos/${id}`, this.todo)
      .then( response => (this.todos = response.data))
    },
    getTodos: function(){

    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
