<template>
  <form id="task-form">
    <InputField />
    <button id="clear-btn" @click="deleteDoneTodos">remove</button>
    <RadioFilter />
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
<style scoped>
#task-form {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  align-items: center;
  justify-content: space-between;
}
</style>
