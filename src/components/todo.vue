<template>
  <div class="todo">

    <v-container fluid>
      <v-layout row wrap>
        <v-text-field v-model="newTitle" label="Todo's title" class="mr-5"></v-text-field>
        <v-text-field v-model="newText" label="What you want to do" class="mr-2"></v-text-field>
        <v-btn dark color="teal lighten-1" @click="addTodo">Add to list</v-btn>
      </v-layout>
    </v-container>

  <v-container fluid>
    <v-list v-if="todos.length !== 0" two-line light>
      <template v-for="(todo, index) in todos">
        <v-list-tile
          :key="todo.title"
          avatar
        >
          <v-list-tile-avatar>
            <v-checkbox v-model="todo.isDone"></v-checkbox>
          </v-list-tile-avatar>
          <v-list-tile-content> 
            <v-list-tile-title v-bind:class="[todo.isDone ? 'green--text' : '']"> {{todo.title}}</v-list-tile-title>
            <v-list-tile-sub-title>{{ todo.text }}</v-list-tile-sub-title>
            <v-btn small dark absolute color="teal" depressed fab right @click="removeTodo(index)">
              <v-icon>delete_outline</v-icon>
            </v-btn> 
          </v-list-tile-content>
        </v-list-tile>
      </template>
    </v-list>
    <h1 class="text-xs-center headline" v-else>Your list is empty. Good job</h1>
  </v-container>
    

  </div>
</template>

<script>

  export default {
    name: 'todo',
    data() {
      return {
        newTitle: '',
        newText: '',
        todos: [
          {
            title: 'Book',
            text: 'read a book',
            isDone: false
          }
        ]
      }
    },
    methods: {
      addTodo: function () {
        if (this.newTitle && this.newText) {
          if (this.todos.find(el => el.title === this.newTitle) === undefined) {
            this.todos.push({
            title: this.newTitle,
            text: this.newText,
            isDone: false
          })
          this.newTitle = '';
          this.newText = '';
          }
        }
      },
      removeTodo: function (index) {
        this.todos.splice(index, 1);
      }
    }
  }

</script>