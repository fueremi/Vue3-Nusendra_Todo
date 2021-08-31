<template>
  <ul class="list-group">
    <li class="list-group-item" v-for="(todo, index) in todos" :key="index">
      <div
        class="d-flex align-items-center justify-content-between"
        :class="todo.isDone ? 'text-decoration-line-through' : ''"
      >
        <p class="m-0 flex-grow-1">{{ todo.activity }}</p>
        <div class="buttons d-flex gap-2">
          <button class="btn btn-sm" @click="doneTodo(index)">
            <i class="fas fa-check-circle text-primary"></i>
          </button>
          <button
            class="btn btn-sm"
            :class="todo.isDone ? 'disabled' : ''"
            @click="deleteTodo(index)"
          >
            <i
              class="fas fa-trash-alt"
              :class="todo.isDone ? 'text-secondary' : 'text-danger'"
            ></i>
          </button>
        </div>
      </div>
    </li>
  </ul>
</template>

<script>
export default {
  props: {
    todos: {
      type: Array,
      default: [],
    },
  },
  setup(props, { emit }) {
    const deleteTodo = (index) => {
      emit("deleteTodo", index);
    };

    const doneTodo = (index) => {
      emit("doneTodo", index);
    };

    return { deleteTodo, doneTodo };
  },
};

//   name: "List",
//   props: {
//     todos: {
//       type: Array,
//       default: [],
//     },
//   },
//   methods: {
//     deleteTodo(i) {
//       this.$emit("deleteTodo", i);
//     },
//     doneTodo(i) {
//       this.$emit("doneTodo", i);
//     },
//   },
// };
</script>
