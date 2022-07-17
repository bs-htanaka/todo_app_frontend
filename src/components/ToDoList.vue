<template>
  <div>
    <h2>ToDO List</h2>
    <div v-if="!isEditing">
      <input type="input" v-model='todo'>
      <button @click='addTodo'>ADD</button>
    </div>
    <div v-else>
      <input type="input" v-model='todo.name'>
      <button @click='updateTodo'>update</button>
    </div>
    <div>
      <ol>
        <li v-for="(todo, index) in todos" :key="index">
          <span> {{ todo.name }} </span>

          <button @click="editTodo(index, todo)">edit</button>
          <button @click="deleteTodo(index)">delete</button>
        </li>
      </ol>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'ToDoList',
  data() {
    return {
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
      this.todos.push(todo)
      sessionStorage.setItem(this.storageKey, JSON.stringify(this.todos));
      this.todo = ''
    },
    editTodo: function (index, todo) {
      this.todo = todo
      this.selectedIndex = index
      this.isEditing = true
    },
    updateTodo: function () {
      this.todos.splice(this.selectedIndex, 1, this.todo)
      sessionStorage.setItem(this.storageKey, JSON.stringify(this.todos));
      this.isEditing = false
      this.todo = ''
    },
    deleteTodo: function (index) {
      this.todos.splice(index, 1);
      sessionStorage.setItem(this.storageKey, JSON.stringify(this.todos));
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
