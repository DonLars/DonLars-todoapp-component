<template>
  <ol class="task-list">
    <ListItem
      v-for="todo in items"
      :key="todo.id"
      :todo="todo"
      @updateDoneTodo="updateTodo"
    />
  </ol>
</template>

<script>
import ListItem from "@/components/ListItem.vue";

export default {
  name: "TodoList",
  emits: ["updateDoneTodo"],

  components: {
    ListItem,
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
  },
};
</script>

<style scoped>
.task-list {
  padding-left: 0;
}
</style>
