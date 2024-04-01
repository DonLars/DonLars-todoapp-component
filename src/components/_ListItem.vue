<template>
  <li class="task-item" v-for="todo of filteredTodos" :key="todo.id">
    <div class="checkbox-wrapper">
      <input
        type="checkbox"
        name="item-1"
        id="'item-' + todo.id"
        :value="todo.id"
        :checked="todo.done"
        @change="updateTodo(todo)"
      />
      <label :for="'item-' + todo.id">{{ todo.description }}</label>
      <button class="delete-task" @click="deleteTodo(todo.id)">X</button>
    </div>
  </li>
</template>
<script>
export default {
  name: "TodoList",
  emits: ["updateDoneTodo"],
};
</script>
<style scoped></style>
