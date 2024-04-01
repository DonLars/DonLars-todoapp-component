<template>
  <form id="task-form">
    <label class="hidden" for="input-field">new task</label>
    <input
      type="text"
      v-model="newTodoText"
      name="input-field"
      id="input"
      placeholder="Start your list!"
      autofocus
    />
    <!-- <p>{{ newTodoText }}</p> -->

    <button @click.prevent="addNewTodo" type="submit">Add</button>
  </form>
</template>

<script>
export default {
  name: "InputField",
  emits: ["addTodo"],

  data() {
    return {
      newTodoText: "",
    };
  },

  methods: {
    addNewTodo() {
      const newTodo = {
        description: this.newTodoText.trim(),
        done: false,
      };
      this.$emit("addTodo", newTodo);

      this.newTodoText = "";
    },
  },
};
</script>
<style scoped>
.hidden {
  position: absolute;
  left: -9999px;
}
input {
  border: 5px solid var(--gradient1-color);
  font-size: 2rem;
  padding: 1rem;
  border-radius: 1rem;
  font-family: monospace;
}

input:focus {
  outline: 0;
  border: 5px solid var(--gradient2-color);
}

#task-form input {
  margin-right: 1rem;
}
</style>
