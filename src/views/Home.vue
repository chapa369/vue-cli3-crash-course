<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
    
  </div>
</template>

<script>
import Todos from '../components/Todos.vue';
import AddTodo from '../components/AddTodo.vue';
import axios from 'axios';
import { error } from 'util';


export default {
  name: 'Home',
  components: {
   Todos,
   AddTodo
   },

methods:{
  deleteTodo(id){
    axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
    .then(res=>this.todos=this.todos.filter(todo=>todo.id!==id))
    .catch(err=>console.log(err));
    this.todo = this.todos.filter(todo=>todo.id !== id);
  },
  addTodo(newTodo){
    const {title, completed} = newTodo;
    axios.post('https://jsonplaceholder.typicode.com/todos',{
      title,
      completed
    })
    .then(res=>this.todos = [...this.todos,res.data])
    .catch(err=>consol.log(err));

    },
},
created(){
  console.log("success create");
  axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5')
  .then(res=>{this.todos = res.data;
  console.log(res);
  })
  .catch(err=>console.log(err));
},


  data(){
    return {
        todos: []
    }

   }
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

*{
  box-sizing: border-box;
  margin:0;
  padding: 0;
}
</style>
