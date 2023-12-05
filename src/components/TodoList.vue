<template>
  <div>
    <div>
      <input
        type="text"
        class="todo-input"
        placeholder="what needs to be done"
        v-model="newtodo"
        @keyup.enter="addtodos"
      />
      <button id="Add-item" @click="AddTodo(index)">Add</button>
    </div>

    <transition-group
      name="fade"
      enter-active-class="animated fadeInup"
      leave-active-class="animated fadeOutDown"
    >
      <div
        v-for="(todo, index) in todosFiltered"
        :key="todo.id"
        class="todo-item"
      >
        <div class="todo-item-left">
          <input type="checkbox" v-model="todo.completed" />

          <div
            v-if="!todo.editing"
            @dblclick="editTodo(todo)"
            class="todo-item-label"
            :class="{ completed: todo.completed }"
          >
            {{ todo.title }}
          </div>
          <input
            v-else
            class="todo-item-edit"
            type="text"
            v-model="todo.title"
            @blur="doneEdit(todo)"
            @keyup.enter="doneEdit(todo)"
            @keyup.esc="cancelEdit(todo)"
            v-focus
          />

          <div class="edit-btn" @click="editTodo(todo)">
            <button class="edit-btn">Edit</button>
          </div>
        </div>

        <div class="remove-item" @click="removeTodo(index)">
          <button class="del-btn">delete</button>
        </div>
      </div>
    </transition-group>

    <div class="extra-container">
      <div>
        <label
          ><input
            type="checkbox"
            :checked="!anyRemaining"
            @change="CheckAllTodos"
          />Check All</label
        >
      </div>
      <div>{{ remaining }} items left</div>
    </div>
    <div class="extra-container">
      <div>
        <button :class="{ active: filter == 'all' }" @click="filter = 'all'">
          All
        </button>
        <button
          :class="{ active: filter == 'active' }"
          @click="filter = 'active'"
        >
          Active
        </button>
        <button
          :class="{ active: filter == 'completed' }"
          @click="filter = 'completed'"
        >
          Completed
        </button>
      </div>

      <div>
        <transition name="fade">
          <button v-if="showClearCompletedButton" @click="ClearCompleted">
            Clear Completed
          </button>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "todo-list",
  data() {
    return {
      newtodo: "",
      idForTodo: 3,
      beforeEditCache: "",
      filter: "all",
      todos: [
        {
          id: 1,
          title: "Character is power",
          completed: false,
          editing: false,
        },
        {
          id: 2,
          title: "Success always leaves footprints",
          completed: false,
          editing: false,
        },
      ],
    };
  },

  computed: {
    remaining() {
      return this.todos.filter((todo) => !todo.completed).length;
    },
    anyRemaining() {
      return this.remaining != 0;
    },

    todosFiltered() {
      if (this.filter == "all") {
        return this.todos;
      } else if (this.filter == "active") {
        return this.todos.filter((todo) => !todo.completed);
      } else if (this.filter == "completed") {
        return this.todos.filter((todo) => todo.completed);
      }

      return this.todos;
    },

    showClearCompletedButton() {
      return this.todos.filter((todo) => todo.completed).length > 0;
    },
  },

  directives: {
    focus: {
      inserted: function (el) {
        el.focus();
      },
    },
  },

  methods: {
    addtodos() {
      if (this.newtodo.trim().length == 0) {
        return;
      }

      this.todos.push({
        id: this.idForTodo,
        title: this.newtodo,
        completed: false,
      });

      this.newtodo = "";
      this.idForTodo++;
    },
    editTodo(todo) {
      this.beforeEditCache = todo.title;
      todo.editing = true;
    },
    doneEdit(todo) {
      if (todo.title.trim() == "") {
        todo.title = this.beforeEditCache;
      }
      todo.editing = false;
    },

    cancelEdit(todo) {
      todo.title = this.beforeEditCache;
      todo.editing = false;
    },

    removeTodo(index) {
      this.todos.splice(index, 1);
    },

    AddTodo(index) {
      if (this.newtodo.trim().length == 0) {
        return;
      }

      this.todos.splice(index, 0, {
        id: this.idForTodo,
        title: this.newtodo,
        completed: false,
      });

      this.newtodo = "";
      this.idForTodo++;
    },

    CheckAllTodos() {
      this.todos.forEach((todo) => (todo.completed = event.target.checked));
    },

    ClearCompleted() {
      this.todos = this.todos.filter((todo) => !todo.completed);
    },
  },
};
</script>

<style lang="scss">
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css");

.todo-input {
  width: 70%;
  padding: 10px 18px;
  font-size: 20px;
  border-radius: 5px;
  margin-bottom: 16px;
  color: white;

  &:focus {
    outline: 0;
  }
}

.todo-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  animation-duration: 0.3s;
  color: white;
}

.remove-item {
  cursor: pointer;
  margin-left: 14px;
  &:hover {
    color: black;
  }
}
#Add-item {
  position: absolute;
  cursor: pointer;
  margin-left: 6px;
  float: right;
  background-color: black;
  color: white;

  &:hover {
    background-color: blue;
  }
}

.todo-item-right {
  display: flex;
  align-items: center;
}

.todo-item-checkbox {
  margin-right: 10px;
}

.todo-item-checkbox-label {
  margin-right: 10px;
}

.todo-item-left {
  display: flex;
  align-items: center;
}

.todo-item-label {
  padding: 10px;
  margin-left: 12px;
}

.todo-item-edit {
  font-size: 24px;
  color: #2c3e50;
  margin-left: 12px;
  background-color: white;
  width: 100%;
  padding: 10px;
  border: 2px solid black;
  font-family: Arial, Helvetica, sans-serif;

  &:focus {
    outline: none;
  }
}
.edit-btn {
  margin-left: 42px;
  padding: 10px;
  color: white;

  &:focus {
    outline: none;
  }
}

.completed {
  text-decoration: line-through;
  color: gray;
}

.extra-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 16px;
  border-top: 1px solid lightgray;
  padding-top: 14px;
  margin-bottom: 14px;
  color: white;
}

button {
  font-size: 19px;
  background-color: black;
  color: white;
  appearance: none;
  border: 1px solid black;
  border-radius: 4px;
  padding: 10px 16px;
  margin-left: 12px;
  cursor: pointer;

  &:hover {
    background-color: blue;
  }

  &:focus {
    outline: none;
  }
}
.active {
  background-color: blue;
}

.del-btn {
  background-color: red;
  color: white;
}

.del-btn:hover {
  background-color: darkred;
}

.todo-item-right {
  display: flex;
  align-items: center;
}

.todo-item-checkbox {
  margin-right: 10px;
}

.todo-item-checkbox-label {
  margin-right: 10px;
}

//css transitions
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
