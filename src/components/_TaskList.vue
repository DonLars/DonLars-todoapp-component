<template>
  <ol class="task-list">
    <ListItem
      v-for="todo in filteredTodos"
      :key="todo.id"
      :todo="todo"
      @updateDoneTodo="updateTodo"
    />
  </ol>
</template>

<script>
import ListItem from "@/components/ListItem.vue";

export default {
  name: "TaskList",
  components: {
    ListItem,
  },
};
</script>

<style scoped>
.task-list {
  padding-left: 0;
}
</style>
