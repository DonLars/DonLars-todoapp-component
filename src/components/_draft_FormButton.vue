<template>
  <button @click.prevent="handleButtonClick" :type="buttonType">
    <slot></slot>
  </button>
  <!--<button id="add-btn" @click.prevent="addTodo()" type="submit">add</button>
  <button id="clear-btn" @click="deleteDoneTodos">remove</button>
  <button class="delete-task" @click="deleteTodo(todo.id)">X</button>-->
</template>
<script>
export default {
  props: {
    buttonType: String, // Type of buttons ('submit', 'deleteTodo', 'deleteDoneTodos'
  },
  methods: {
    handleButtonClick() {
      switch (this.buttonType) {
        case "submit":
          this.addTodo();
          break;
        case "deleteTodo":
          this.deleteTodo();
          break;
        case "deleteDoneTodos":
          this.deleteDoneTodos();
          break;
        default:
          break;
      }
    },
  },
  addTodo() {},
  deleteTodo() {},
  deleteDoneTodos() {},
};
</script>
<style>
button {
  text-transform: uppercase;
  border: 5px solid var(--gradient2-color);
  border-radius: 0.5rem;
  background: black;
  color: var(--white-color);
  font-family: monospace;
  padding: 1rem;
  font-size: 2rem;
  cursor: pointer;
}
button:hover,
button:active,
button:focus,
select:hover,
select:active,
select:focus {
  border: 5px solid var(--gradient1-color);
}
</style>
