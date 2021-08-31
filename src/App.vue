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
            v-if="list.length > 0"
            :todos="list"
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
import { ref, reactive, onMounted, toRefs, computed } from "vue";

export default {
  components: {
    List,
  },
  setup() {
    // ? Data
    const todo = ref("");
    const todos = reactive({
      list: [],
    });

    // ? computed: {}
    const totalTodo = computed(() => {
      return todos.list.length;
    });

    // ? methods: {}
    const addTodo = () => {
      todos.list.unshift({
        activity: todo.value,
        isDone: false,
      });
      todo.value = "";
      saveToLocalStorage();
    };

    const onDelete = (i) => {
      todos.list = todos.list.filter((item, index) => {
        if (index !== i) {
          return item;
        }
      });
      saveToLocalStorage();
    };

    const onDone = (i) => {
      todos.list = todos.list.filter((item, index) => {
        if (index === i) {
          item.isDone = !item.isDone;
        }
        return item;
      });
      saveToLocalStorage();
    };

    const saveToLocalStorage = () =>
      localStorage.setItem("todos", JSON.stringify(todos.list));

    // ? mounted() {}
    onMounted(() => {
      const items = localStorage.getItem("todos");
      todos.list = items ? JSON.parse(items) : [];
    });

    return {
      todo,
      ...toRefs(todos),
      totalTodo,
      addTodo,
      onDone,
      onDelete,
    };
  },

  // name: "App",
  // data() {
  //   return {
  //     todo: "",
  //     todos: [],
  //   };
  // },
  // components: {
  //   List,
  // },
  // computed: {
  //   totalTodo() {
  //     return this.todos.length;
  //   },
  // },
  // methods: {
  //   addTodo() {
  //     this.todos.unshift({
  //       activity: this.todo,
  //       isDone: false,
  //     });
  //     this.saveToLocalStorage();
  //     this.todo = "";
  //   },
  //   onDelete(i) {
  //     this.todos = this.todos.filter((item, index) => {
  //       if (index !== i) {
  //         return item;
  //       }
  //     });
  //     this.saveToLocalStorage();
  //   },
  //   onDone(i) {
  //     this.todos = this.todos.filter((item, index) => {
  //       if (index === i) {
  //         item.isDone = !item.isDone;
  //       }

  //       return item;
  //     });
  //     this.saveToLocalStorage();
  //   },
  //   saveToLocalStorage() {
  //     localStorage.setItem("todos", JSON.stringify(this.todos));
  //   },
  // },
  // mounted() {
  //   this.todos = JSON.parse(localStorage.getItem("todos"));
  // },
};
</script>
