<template>
  <div id="app">
    <Header />
    <addTodo v-on:add-todo="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>

<script>
import Todos from './components/Todos';
import Header from './components/layouts/header';
import addTodo from './components/addTodo';
import axios from 'axios';
export default {
  name: 'app',
  components: {
    Header,
    addTodo,
    Todos
  },
  data(){
    return{
      todos:[]
    }
  },
  methods:{
    deleteTodo(id){
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
      .then(this.todos = this.todos.filter(todo => todo.id !== id))
      .catch();
    },
    addTodo(newTodo){
      const {title, completed } = newTodo;
      axios.post('https://jsonplaceholder.typicode.com/todos', {
        title, completed
      })
      .then(res => this.todos = [...this.todos, res.data])
      .catch();
      this.todos = [...this.todos, newTodo];
    }
  },
  created(){
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=10')
    .then(res =>  this.todos = res.data)
    .catch();
  }
}
</script>

<style>
*{
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body{
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}
.btn{
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}

.btn:hover{
  background: #666;
}
</style>
