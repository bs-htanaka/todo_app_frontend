<template>
  <div>
    <v-card class="mx-auto" max-width="500">
    <v-toolbar color="purple" dark>
      <v-toolbar-title>ToDo List</v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>

    <v-list subheader three-line>
      <v-form v-if="!isEditing">
        <v-text-field label="Name" v-model='todo'></v-text-field>
        <v-btn @click='addTodo' color="primary" small>ADD</v-btn>
      </v-form>
      <v-form v-else>
        <v-text-field label="Name" v-model='todo.name'></v-text-field>
        <v-btn @click='updateTodo' color="primary" small>update</v-btn>
      </v-form>

      <v-list-item v-for="(todo, index) in todos" :key="index">
        <v-list-item-content>
          <v-list-item-title>{{ todo.name }}</v-list-item-title>
        </v-list-item-content>
        <v-btn @click="editTodo(index, todo)" color="success" small>edit</v-btn>
        <v-btn @click="deleteTodo(todo)" color="error" small>delete</v-btn>
      </v-list-item>
    </v-list>
  </v-card>
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
