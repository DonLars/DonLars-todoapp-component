<template>
  <label class="hidden" for="input-field">new task</label>
  <input
    type="text"
    v-model="newTodoText"
    name="input-field"
    id="input"
    placeholder="Start your list!"
    autofocus
  />
  <button @click.prevent="addNewTodo" type="submit">Add</button>

  <!-- <FormButton buttonType="submit">add</FormButton> -->
</template>
<script>
export default {
  name: "InputField",
  components: {},
};
</script>
<style>
.hidden {
  position: absolute;
  left: -9999px;
}

input[type="checkbox"]:checked + label {
  text-decoration: line-through;
  font-style: italic;
  color: var(--gradient1-color);
}

.task-item:has(input[type="checkbox"]:checked) {
  opacity: 0.4;
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
</style>
