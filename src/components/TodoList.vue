<template>
  <div class="hello">
    <input type="text" class="todo-input" placeholder="Enter your task" v-model="newTodo" @keyup.enter="addTodo">
    <div v-for="(todo, index) in todosFiltered" :key="todo.id" class="todo-item">
      <div class="todo-item-left">
        <input type="checkbox" v-model="todo.completed">
        <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" :class="{ completed : todo.completed }">{{ todo.title }}</div>
        <input v-else type="text" class="todo-item-edit" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
      </div>
      <div class="remove-item" @click="removeTodo(index)">
        &times;
      </div>
    </div>
    <div class="extra-container">
      <div>
        <button :class="{ avtive: filter == 'all' }" @click="filter = 'all'">All</button>
        <button :class="{ avtive: filter == 'active' }" @click="filter = 'active'">Active</button>
        <button :class="{ avtive: filter == 'completed' }" @click="filter = 'completed'">Completed</button>
      </div>
      <div>
        <button v-if="showClearCompletedB" @click="clearCompleted">Clear Completed</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'todo-list',
  data () {
    return {
      newTodo: '',
      idForTodo: 3,
      beforeEditcache: '',
      filter: 'all',
      todos:[
        {
          'id' : 1,
          'title' : 'Finish task',
          'completed' : false,
          'editing' : false,
        },
        {
          'id' : 2,
          'title' : 'Finish task',
          'completed' : false,
          'editing' : false,

        },
      ]
    }
  },
  computed: {
    todosFiltered(){
      if (this.filter == 'all') {
        return this.todos
      }else if(this.filter == 'active'){
        return this.todos.filter(todo => !todo.completed)
      }else if(this.filter == 'completed'){
        return this.todos.filter(todo => todo.completed)
      }
      return this.todos
    },
    showClearCompletedB(){
      return this.todos.filter(todo => todo.completed).length > 0
    }
  },
  directives:{
    focus: {
      inserted: function(el){
        el.focus()
      }
    }
  },
  methods:{
    addTodo(){
      if(this.newTodo.trim().length == 0){
        return
      }
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
      })
      this.newTodo = ''
      this.idForTodo++
    },
    editTodo(todo){
      this.beforeEditcache = todo.title
      todo.editing = true
    },
    doneEdit(todo){
      todo.editing = false
    },
    cancelEdit(todo){
      todo.title = this.beforeEditcache,
      todo.editing = false
    },
    removeTodo(index){
      this.todos.splice(index, 1)
    },
    clearCompleted(){
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
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

.todo-input{
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;
}

.todo-item{
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.remove-item{
  cursor: pointer;
  margin-left: 14px;
  &:hover{
    color: black;
  }
}

.todo-item-left{
  display: flex;
  align-items: center;
}

.todo-item-label{
  padding: 10px;
  border: 1px solid white;
  margin-left: 12px;
}

.todo-item-edit{
  font-size: 24px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
}

.completed {
  text-decoration: line-through;
  color: grey;
}

</style>
