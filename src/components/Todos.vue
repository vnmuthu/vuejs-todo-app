<template>
  <div class = "todos container">
    <div class="row header">
        <h1>{{ todoTitle }}</h1>
    </div>
    <div class="row">
      <form @submit.prevent="submitTodo" class="">
        <input class="new-todo" autofocus autocomplete="off" placeholder="What needs to be done?" v-model="newTodo" @keyup.enter="submitTodo" @keydown="noAlertMsg" size="60" height="30" />
        <button class="btn btn-success">+</button>
        <div class="alert" role="alert">{{ this.noTodoMsg }}</div>
      </form>
    </div>
    <div class="row">
      <ul class="collection">
        <li class="collection-item" v-for="todo in todos" :key="todo.id">
          <div class="todoCheckBox"><input type="checkbox" :checked=todo.done @change="todo.done = !todo.done" /></div>
          <div class="todoContent" v-bind:class="{ strikeTodo: todo.done }">{{todo.title}}</div>
          <div class="todoDelete">
            <a @click.prevent="deleteTodo(todo)">
              <span class="delete-todo">delete</span>
            </a>
          </div>
        </li>
      </ul>
      <div class="todoSummary">
        <p class="text-left text-info">Total - {{ todos.length }}</p>
        <p class="text-right text-success">Done - {{ completedTodos }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Todos',
  data() {
      return {
        todoTitle: 'My To-Do',
        todos: [],
        newTodo: '',
        noTodoMsg: ''

      };
    },
    computed: {
      completedTodos: function() {
        var completeTodo = 0;

        if (this.todos.length > 0) {
          this.todos.filter(function(todo) {
            if(todo.done) completeTodo++;
          });
        }

        return completeTodo;
      }
    },
    watch: {
      todos: {
        handler() {
          localStorage.todos = JSON.stringify(this.todos);
        },
        deep: true,
      },
    },
    mounted() {
      if (localStorage.todos) {
        this.todos = JSON.parse(localStorage.todos);
      }
    },
    methods: {
      submitTodo() {
        if(this.newTodo) {
          this.todos.push({
            title: this.newTodo,
            done: false,
          });
          this.newTodo = '';
          this.noTodoMsg = '';
        } else {
          this.noTodoMsg = 'Please enter todo text before submit';
        }

      },
      deleteTodo(todo) {
        const todoIndex = this.todos.indexOf(todo);
        this.todos.splice(todoIndex, 1);
      },
      noAlertMsg() {
        this.noTodoMsg = '';
      }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
input {
    top: 5px;
    line-height: 2em;
}
.strikeTodo {
  text-decoration: line-through;
}
.alert {
  color: red;
}
.collection {
  margin-top: 30px;
  padding: 0;
}
.collection-item {
  width: 100%;
  height: 40px;
  display: block;
  padding: 10px 0;
  box-sizing: border-box;
  background: #ededed;
  border-bottom: 1px solid #fff;
}
.todoCheckBox, .todoContent, .todoDelete {
  float: left;
}
.todoCheckBox {
  width: 10%;
}
.todoContent {
  width: 80%;
  text-align: left;
}
.todoDelete {
  width: 10%;
}
.delete-todo {
  cursor: pointer;
}
.todoSummary {
  display: inline-block;
  font-weight: bold;
}
</style>
