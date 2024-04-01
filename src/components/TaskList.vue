<template>
  <ul class="task-list">
    <TodoListItem
      v-for="todo in items"
      :key="todo.id"
      :todo="todo"
      @updateDoneTodo="updateTodo"
    />
  </ul>
</template>

<script>
import TodoListItem from "@/components/TodoListItem.vue";

export default {
  name: "TaskList",
  emits: ["updateDoneTodo", "deleteTodo"],

  components: {
    TodoListItem,
  },

  props: {
    items: {
      type: Array,
      default() {
        return [];
      },
    },
  },

  methods: {
    updateTodo(clickedTodo) {
      this.$emit("updateDoneTodo", clickedTodo);
    },
    deleteTodo(todoId) {
      this.$emit("deleteTodo", todoId);
    },
  },
};
</script>
<style>
.task-list {
  padding-left: 0;
}

.task-list {
  padding-left: 0;
}
.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
  margin: 1rem 0;
  padding: 0.5rem;
  background: linear-gradient(0deg, #ddd 50%, #fff 50%);
  border-radius: 1rem;
  border: 5px solid var(--gradient2-color);
  color: var(--black-color);
  font-size: 2rem;
  transition: 0.5s;
}

.checkbox-wrapper {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 2rem;
}
</style>
