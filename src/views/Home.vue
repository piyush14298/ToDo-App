<template>
  <div id="app">
    <h3> Enter your user ID to view your TODOs</h3>
    <Input v-on:getuser="getuser" />
    <h3><br><br>Want to add to your TODOs? Enter your ID and the TODO</h3>
    <AddTodo v-on:add-todo="addTodo" />
    <br><br><br><h2>TODOs</h2>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Todos from '../components/Todos';
import AddTodo from '../components/AddTodo';
import Input from '../components/Input';
import axios from 'axios';

export default {
  name: 'Home',
  components: {
    Input,
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: []
    }
  },
  methods: {
    deleteTodo(id) {
      axios.delete(`http://localhost:3000/posts/${id}`)
        .then(res => this.todos = this.todos.filter(todo => todo.id !== id))
        // eslint-disable-next-line no-console
        .catch(err => console.log(err));
    },
    addTodo(newTodo) {
      const {userId, title, completed } = newTodo;

      axios.post('http://localhost:3000/posts', {
        userId,
        title,
        completed
      })
        .then(res => this.todos = [...this.todos, res.data].filter(todo=> todo.userId == this.userId))
        // eslint-disable-next-line no-console
        .catch(err => console.log(err));
    },
    getuser(userId) {
      this.todos=axios.get('http://localhost:3000/posts?')
      .then(res => this.todos = res.data.filter(todo => todo.userId == userId))
      // eslint-disable-next-line no-console
      .catch(err => console.log(err));
    }
  },
  // created() {
  //   axios.get('http://localhost:3000/posts?')
  //     .then(res => this.todos = res.data)
  //     // eslint-disable-next-line no-console
  //     .catch(err => console.log(err));
  // }
}
</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body {
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.4;
  }

  .btn {
    display: inline-block;
    border: none;
    background: #555;
    color: #fff;
    padding: 7px 20px;
    cursor: pointer;
  }

  .btn:hover {
    background: #666;
  }
</style>
