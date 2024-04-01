<template>
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <meta name="author" content="Lars Wagner" />
      <meta name="description" content="First TODO-App" />
      <title>My 1st TODO App with Vue</title>
    </head>
    <body>
      <main>
        <article class="todo-article" id="app">
          <h1>My Todo-App with Vue</h1>
          <h2 class="claim">Getting things done…</h2>

          <pre>
          {{ $data }}
        </pre
          >
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

            <button id="add-btn" @click.prevent="addTodo()" type="submit">
              add
            </button>
            <button id="clear-btn" @click="deleteDoneTodos">remove</button>

            <div class="filter">
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
            </div>
          </form>

          <ul class="task-list">
            <li class="task-item" v-for="todo of filteredTodos" :key="todo.id">
              <div class="checkbox-wrapper">
                <input
                  type="checkbox"
                  name="item-1"
                  id="'item-' + todo.id"
                  :checked="todo.done"
                  @change="updateTodo(todo)"
                />
                <label :for="'item-' + todo.id">{{ todo.description }}</label>
                <button class="delete-task" @click="deleteTodo(todo.id)">
                  X
                </button>
              </div>
            </li>
          </ul>
        </article>
        <!--         <TaskForm />
        <TaskList /> -->
      </main>
    </body>
  </html>
</template>

<script>
export default {
  name: "HomeView",
  props: {},

  data() {
    return {
      todos: [],
      newTodoText: "",
      apiUrl: "http://localhost:4730/todos/",
      filter: "all", // Initial filter "all", "open", "done"
    };
  },
  computed: {
    filteredTodos() {
      // selected filter
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
  methods: {
    addTodo() {
      const newTodoObject = {
        description: this.newTodoText.trim(),
        done: false,
      };

      const isDuplicate = this.todos.some(
        (task) =>
          task.description.toLowerCase() ===
          this.newTodoText.trim().toLowerCase()
      );

      if (this.newTodoText === "") {
        alert("Write something down!");
        this.newTodoText = "";
      } else if (isDuplicate) {
        alert("You can't add a duplicate task!");
        this.newTodoText = "";
      } else {
        fetch(this.apiUrl, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(newTodoObject),
        })
          .then((response) => response.json())
          .then((jsonData) => {
            this.todos.push(jsonData);
            this.newTodoText = "";
          });
      }
    },
    updateTodo(todo) {
      const cloneCurrentTodo = {
        id: todo.id,
        description: todo.description,
        done: !todo.done,
      };
      fetch(this.apiUrl + cloneCurrentTodo.id, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(cloneCurrentTodo),
      })
        .then((response) => response.json())
        .then(() => {
          todo.done = !todo.done;
        })
        .catch((error) => {
          console.error(
            "Fehler beim Aktualisieren des Todos in der API:",
            error
          );
        });
    },
    deleteTodo(todoId) {
      fetch(this.apiUrl + todoId, {
        method: "DELETE",
      })
        .then(() => {
          this.todos = this.todos.filter((todo) => todo.id !== todoId);
        })
        .catch((error) => {
          console.error("Fehler beim Löschen des Todos:", error);
        });
    },

    deleteDoneTodos() {
      if (confirm("Do you really want to delete all completed tasks?")) {
        // Filter out completed todos from the current state
        const doneTodos = this.todos.filter((todo) => todo.done);
        if (doneTodos.length === 0) {
          // No done tasks
          console.log("No completed tasks to delete");
          return;
        }

        // Use Promise.all for multiple API requests and delete many completed todos
        Promise.all(
          doneTodos.map((doneTodo) =>
            fetch(this.apiUrl + `/${doneTodo.id}`, {
              method: "DELETE",
              headers: { "Content-Type": "application/json" },
            })
          )
        )
          .then(() => {
            // Update the local state by filtering out completed todos
            this.todos = this.todos.filter((todo) => !todo.done);
          })
          .catch((error) =>
            console.error("Error removing done todos from API:", error)
          );
      }
    },
  },
  created() {
    fetch(this.apiUrl)
      .then((response) => response.json())
      .then((jsonData) => (this.todos = jsonData))
      .catch((error) => console.error("Fehler beim Laden von Todos:", error));
    console.log(this.todos);
  },
};
</script>

<style>
#task-form {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  align-items: center;
  justify-content: space-between;
}
.task-list {
  padding-left: 0;
}

/* Input */

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

/* List Item */

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

/* Filter */

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
  cursor: pointer;
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
  cursor: pointer;

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
