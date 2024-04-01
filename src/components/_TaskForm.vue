<template>
  <form id="task-form">
    <InputField v-model="newTodoText" @enter="addTodo" />
    <button id="clear-btn" @click="deleteDoneTodos">Remove completed</button>
    <RadioFilter v-model="filter" />
  </form>
</template>

<script>
import InputField from "@/components/InputField.vue";
import RadioFilter from "@/components/RadioFilter.vue";

export default {
  name: "TaskForm",
  components: {
    InputField,
    RadioFilter,
  },
};
</script>

<style scoped></style>
