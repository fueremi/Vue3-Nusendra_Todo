<template>
  <div class="container mt-2">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">Simple Todo App</h5>
        <div class="row">
          <div class="col-10">
            <input
              type="text"
              class="form-control"
              v-model="todo"
              @keyup.enter="addTodo"
              placeholder="Input your todo here ..."
            />
          </div>
          <div class="col-2">
            <button class="btn btn-success" @click="addTodo">Add</button>
          </div>
        </div>
        <hr />
        <div class="mt-3">
          <List
            v-if="todos.length > 0"
            :todos="todos"
            @deleteTodo="onDelete"
            @doneTodo="onDone"
          />
          <div
            class="d-flex flex-column align-items-center justify-content-center"
            v-else
          >
            <span>
              <i class="fab fa-xbox text-mute"></i>
            </span>
            <p class="m-0">Todo is Empty</p>
          </div>
          <hr />
          <small>Total Todo: {{ totalTodo }}</small>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import List from "./components/List.vue";

export default {
  name: "App",
  data() {
    return {
      todo: "",
      todos: [],
    };
  },
  components: {
    List,
  },
  computed: {
    totalTodo() {
      return this.todos.length;
    },
  },
  methods: {
    addTodo() {
      this.todos.unshift({
        activity: this.todo,
        isDone: false,
      });
      this.saveToLocalStorage();
      this.todo = "";
    },
    onDelete(i) {
      this.todos = this.todos.filter((item, index) => {
        if (index !== i) {
          return item;
        }
      });
      this.saveToLocalStorage();
    },
    onDone(i) {
      this.todos = this.todos.filter((item, index) => {
        if (index === i) {
          item.isDone = !item.isDone;
        }

        return item;
      });
      this.saveToLocalStorage();
    },
    saveToLocalStorage() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
  },
  mounted() {
    this.todos = JSON.parse(localStorage.getItem("todos"));
  },
};
</script>
