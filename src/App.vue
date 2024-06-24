<template>
    <h1>Tasklist</h1>
    <TaskList/>
</template>

<script>
import TaskList from './components/TaskList.vue'

export default {
  name: 'App',
  components: { TaskList }
}
</script>

<style>
@font-face {
  font-family: 'Roboto';
  src: url(./assets/fonts/Roboto-Regular.ttf);
}

body {
  margin: 0; padding: 0;
  background-color: #151515;
}

#app {
    margin: 60px auto; padding: 0;
    font-family: 'Roboto', sans-serif;
    text-align: center;
    display: flex; flex-direction: column;
}

h1 {
  color: white;
}
</style>
