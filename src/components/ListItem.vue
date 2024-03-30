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

      <!-- <FormButton class="delete-task" buttonType="deleteTodo">X</FormButton> -->
    </div>
  </li>
</template>
<script>
/* import FormButton from "@/components/FormButton.vue"; */

export default {
  name: "TodoList",
  emits: ["updateDoneTodo"],

  components: {},
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
  },
};
</script>
<style scoped>
input:checked ~ span {
  text-decoration: line-through;
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

.checkbox-wrapper label {
  cursor: pointer;
  color: black;
  flex-grow: 2;
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
