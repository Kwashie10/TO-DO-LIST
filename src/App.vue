<template>
  <div id="app" class="container">
    <h1 class="logo">TO-DO-LIST</h1>

    <todo-list></todo-list>
  </div>
</template>

<script>
import TodoList from "./components/TodoList";

export default {
  name: "App",
  components: {
    TodoList,
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  background-color: #06182a;
}

.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 10px;
  text-align: center;
  border: 5px solid blue;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-top: 60px;
  font-size: 24px;
  padding: 10px;
  border-radius: 20px;
}

.logo {
  display: block;
  margin: 20px auto;
  height: 75px;
  color: whitesmoke;
}
</style>
