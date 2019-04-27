<template>
  <v-app>

    <v-content>
      <Todo/>
    </v-content>
  </v-app>
  
</template>

<script>

import Todo from './components/todo'

export default {
  name: 'App',
  components: {
    Todo
  },
  data () {
    return {
      test: 23
    }
  }
}
</script>
