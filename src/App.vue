<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:addTodo="addTodo"></TodoInput>
    <TodoList v-bind:propsdata="todoItems" @removeTodo="removeTodo" @editTodo="editTodo"></TodoList>
    <TodoFooter v-on:removeAll="clearAll"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'

export default {
  data() {
    return {
      todoItems: []
    }
  },
  watch: {
    todoItems: function() {
      this.sortLocalStorage();
    }
  },
  methods: {
    clearAll() {
      localStorage.clear();
      this.todoItems = [];
    },
		addTodo(todoItem) {
			localStorage.setItem(todoItem, todoItem);
      this.todoItems.push(todoItem);
		},
    editTodo(oldTodoItem, newTodoItem, index) {
      localStorage.removeItem(oldTodoItem);
      localStorage.setItem(newTodoItem, newTodoItem);
      this.todoItems.splice(index, 1, newTodoItem);
    },
    removeTodo(todoItem, index) {
      localStorage.removeItem(todoItem);
      this.todoItems.splice(index, 1);
    },
    sortLocalStorage() {
      console.log('localStorageSort 호출');
      let localStorageArr = new Array();
      if (localStorage.length > 0) {
        for (let i=0; i<localStorage.length; i++) {
          localStorageArr[i] = localStorage.key(i)+localStorage.getItem(localStorage.key(i));
        }
      }
      let sortedArr = localStorageArr.sort();
      console.log(sortedArr);
      return sortedArr;
    }
  },
  created() {
		if (localStorage.length > 0) {
			for (var i = 0; i < localStorage.length; i++) {
        if (localStorage.key(i)!=="loglevel:webpack-dev-server") {
          this.todoItems.push(localStorage.key(i));
        }
			}
		}
  },
  components: {
    'TodoHeader': TodoHeader,
    'TodoInput': TodoInput,
    'TodoList': TodoList,
    'TodoFooter': TodoFooter
  }
}
</script>

<style>
  body {
    text-align: center;
    background-color: #F6F6F8;
  }
  input {
    border-style: groove;
    width: 200px;
  }
  button {
    border-style: groove;
  }
  .shadow {
    box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03)
  }
</style>
