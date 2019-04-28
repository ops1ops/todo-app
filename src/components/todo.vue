<template>
  <!-- 
    везде между атрибутом равно и значением пробелы или только при переносах:
    нужно так: class="todo" или class = "todo"
  -->
  <div class="todo">
    <v-container fluid>
      <v-layout row wrap>
        <v-text-field 
          class   = "mr-5"
          v-model = "newTitle" 
          label   = "Todo's title" 
        ></v-text-field>
        <v-text-field 
          class   = "mr-2"
          v-model = "newText" 
          label   = "What you want to do" 
        ></v-text-field>
        <v-btn 
          color     = "teal lighten-1 white--text" 
          @click    = "addTodo"
          :disabled = "!isValid()"
        >Add to list</v-btn> 
        <!-- 
          если текст в теге делать как выше или так:
          <v-btn 
            color  = "teal lighten-1" 
            @click = "addTodo"
            dark
          >
            Add to list
          </v-btn>
        -->
      </v-layout>
      <p 
        class   = "subheading red--text"
        v-show  = "!isEmptyInputs()"
      >Input fields should be filled</p>
      <p 
        class   = "subheading red--text"
        v-show  = "newTitle.length > 0 && !isUniqTitle()"
      >Todo's title should be unique</p>
    </v-container>

    <v-container fluid>
      <p class="headline"> {{ currentStatus }} </p>
      <v-list 
        v-if="todos.length" 
        two-line 
        light
      >
        <template v-for="(todo, index) in todos">
          <v-list-tile
            :key="index"
            avatar
          >
            <v-list-tile-avatar>
              <v-checkbox
                color   = "success" 
                v-model = "todo.isDone" 
              ></v-checkbox>
            </v-list-tile-avatar>
            <v-list-tile-content> 
              <v-list-tile-title :class="[todo.isDone ? 'grey--text' : '']">
                <input 
                  type    = "text" 
                  v-model = "todo.title"
                />
              </v-list-tile-title>
              <v-list-tile-sub-title>
                <input 
                  type    = "text" 
                  v-model = "todo.text"
                />
              </v-list-tile-sub-title>
              <v-btn 
                color="teal" 
                @click="removeTodo(index)"
                small 
                dark 
                absolute 
                depressed 
                fab 
                right 
              >
                <v-icon>delete_outline</v-icon>
              </v-btn> 
            </v-list-tile-content>
          </v-list-tile>
        </template>
      </v-list>
      <h1 
        class = "text-xs-center headline" 
        v-else
      >Your list is empty. Good job</h1>
    </v-container>
  </div>
</template>

<script>
  export default {
    name: 'Todo',
    data() {
      return {
        newTitle  : '',
        newText   : '',
        todos     : [
          {
            title   : 'Book',
            text    : 'read a book',
            isDone  : true
          }
        ]
      }
    },
    methods: {
      isEmptyInputs () {
        if (this.newTitle && this.newText) return true;
      },
      isUniqTitle () {
        if (!this.todos.find(
          el => el.title === this.newTitle
        )) return true;
      },
      isValid () {
        if (this.isEmptyInputs() && this.isUniqTitle()) return true;
      },
      addTodo () {
        if (this.isValid()) {
          this.todos.push({
            title   : this.newTitle,
            text    : this.newText,
            isDone  : false
          });
          this.newTitle = '';
          this.newText  = '';
        }
      },
      removeTodo (index) {
        this.todos.splice(index, 1);
      },
      uncompletedCount () {
        if (this.todos.length > 0) {
          return this.todos.filter(
            el => el.isDone === false
          ).length;
        }
      }
    },
    computed: {
      currentStatus() {
        let uncompletedCount = this.uncompletedCount();
        if (uncompletedCount > 1) {
          return `You have ${ uncompletedCount } uncompleted tasks:`;
        }
        else if (uncompletedCount === 1) {
          return `You have ${ uncompletedCount } uncompleted task:`;
        }
        else if (uncompletedCount === 0) {
          return 'You did everything';
        }   
      }
    }
  }
</script>