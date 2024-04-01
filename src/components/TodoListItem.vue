<template>
  <li class="task-item">
    <div class="checkbox-wrapper">
      <input
        type="checkbox"
        name="todo.id"
        :id="todo.id"
        :for="key"
        :value="todo.id"
        @change="updateTodo"
        :checked="todo.done"
      />
      <label :for="todo.id">
        <span>{{ todo.description }}</span>
      </label>

      <button class="delete-task" @click="deleteTodo">X</button>
    </div>
  </li>
</template>

<style scoped>
input:checked ~ span {
  text-decoration: line-through;
}
</style>

<script>
const apiUrl = "http://localhost:4730";

export default {
  name: "TodoListItem",
  emits: ["updateDoneTodo", "deleteTodo"],

  data() {
    return {
      value: "",
    };
  },

  props: {
    todo: {
      type: Object,
      default() {
        return {};
      },
    },
  },

  methods: {
    updateTodo() {
      const clickedTodo = this.todo;
      this.$emit("updateDoneTodo", clickedTodo);
    },
    async deleteTodo() {
      try {
        await fetch(`${apiUrl}/todos/${this.todo.id}`, {
          method: "DELETE",
        });
        this.$emit("deleteTodo", this.todo.id); // Nur die ID übermitteln
      } catch (error) {
        console.error("Fehler beim Löschen des Todos:", error);
      }
    },
  },
};
</script>
<style>
.checkbox-wrapper label {
  cursor: pointer;
  color: black;
  flex-grow: 2;
}

input[type="checkbox"]:checked + label {
  text-decoration: line-through;
  font-style: italic;
  color: var(--gradient1-color);
}

.task-item:has(input[type="checkbox"]:checked) {
  opacity: 0.4;
}

/* Checkbox */

input[type="checkbox"] {
  appearance: none;
  background-color: black;
  margin: 0;
  font: inherit;
  color: currentColor;
  border: 5px solid var(--gradient2-color);
  border-radius: 0.25em;
  display: grid;
  place-content: center;
  height: 5rem;
  width: 5rem;
  cursor: pointer;
}

input[type="checkbox"]::before {
  content: "";
  width: 1em;
  height: 1em;
  clip-path: polygon(14% 44%, 0 65%, 50% 100%, 100% 16%, 80% 0%, 43% 62%);
  transform: scale(0);
  transform-origin: bottom left;
  transition: 120ms transform ease-in-out;
  box-shadow: inset 1em 1em white;
}

input[type="checkbox"]:checked::before {
  transform: scale(1);
}
</style>
