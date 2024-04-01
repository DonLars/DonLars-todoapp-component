<template>
  <form class="filter" @change="$emit('filterChange', filter)">
    <label for="filter-all">
      <input
        type="radio"
        name="filter"
        id="filter-all"
        value="all"
        v-model="filter"
        checked
      />
      all
    </label>
    <label for="filter-open">
      <input
        type="radio"
        name="filter"
        id="filter-open"
        value="open"
        v-model="filter"
      />
      open
    </label>
    <label for="filter-done">
      <input
        type="radio"
        name="filter"
        id="filter-done"
        value="done"
        v-model="filter"
      />
      done
    </label>
  </form>
</template>
<script>
export default {
  computed: {
    filteredTodos() {
      // selected filter
      console.log(this.todos, this.filter);
      if (this.filter === "open") {
        return this.todos.filter((todo) => !todo.done);
      } else if (this.filter === "done") {
        return this.todos.filter((todo) => todo.done);
      } else {
        // "all" or another filter
        return this.todos;
      }
    },
  },
};
</script>
<style>
/* Radio Filter */

.filter {
  display: flex;
  justify-content: center;
  gap: 1rem;
}
.filter label {
  display: flex;
  align-items: center;
  gap: 1rem;
  font-size: 2rem;
  color: white;
  text-transform: uppercase;
}

input[type="radio"] {
  appearance: none;
  background-color: var(--black-color);
  margin: 0;

  font: inherit;
  color: currentColor;
  width: 1.15em;
  height: 1.15em;
  border: 5px solid var(--gradient2-color);
  border-radius: 50%;
  transform: translateY(-0.075em);

  display: grid;
  place-content: center;
}
input[type="radio"]:hover {
  border: 5px solid var(--gradient2-color);
}

input[type="radio"]::before {
  content: "";
  width: 1rem;
  height: 1rem;
  border-radius: 50%;
  transform: scale(0);
  transition: 120ms transform ease-in-out;
  box-shadow: inset 1em 1em var(--gradient1-color);
}

input[type="radio"]:checked::before {
  transform: scale(1);
}

input[type="radio"]:focus {
  outline: max(2px, 0.15em) solid currentColor;
  outline-offset: max(2px, 0.15em);
}
</style>
