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

          <section class="taskform">
            <InputField @addTodo="addNewTodo" />
            <DeleteButton :items="todos" @deleteDoneTodo="deleteTodo" />
            <TodoFilter @filterChange="(value) => (filter = value)" />
          </section>
          <TaskList
            :items="filteredTodos"
            @updateDoneTodo="updateTodo"
            @deleteTodo="handleDeleteTodo"
          />
        </article>
      </main>
    </body>
  </html>
</template>

<script>
import TodoFilter from "@/components/TodoFilter.vue";
import DeleteButton from "@/components/DeleteButton.vue";
import InputField from "@/components/InputField.vue";
import TaskList from "@/components/TaskList.vue";

const apiUrl = "http://localhost:4730";

export default {
  components: {
    TodoFilter,
    DeleteButton,
    InputField,
    TaskList,
  },

  data() {
    return {
      todos: [
        {
          description: "Learn JS",
          id: 1,
          done: true,
        },

        {
          description: "Learn Vue",
          id: 2,
          done: true,
        },
        {
          description: "Learn Cypress",
          id: 3,
          done: false,
        },
      ],
      filter: "all",
    };
  },

  computed: {
    filteredTodos() {
      console.log(this.todos, this.filter);

      if (this.filter === "all") {
        return this.todos;
      } else if (this.filter === "open") {
        return this.todos.filter((item) => !item.done);
      } else if (this.filter === "done") {
        return this.todos.filter((item) => item.done);
      }
      return this.todos;
    },
  },

  methods: {
    async addNewTodo(newTodo) {
      const isDuplicate = this.todos.some(
        (task) =>
          task.description.toLowerCase() === newTodo.description.toLowerCase()
      );

      if (isDuplicate) {
        alert("You can't add a duplicate task!");
        return;
      }

      if (newTodo.description === "") {
        alert("Write something!");
        return;
      }

      const response = await fetch(`${apiUrl}/todos/`, {
        method: "POST",
        headers: {
          "content-type": "application/json",
        },
        body: JSON.stringify(newTodo),
      });
      const newApiTodo = await response.json();
      console.log(this.todos);
      this.todos.push(newApiTodo);
    },

    async updateTodo(clickedTodo) {
      const updatedTodo = {
        description: clickedTodo.description,
        done: (clickedTodo.done = !clickedTodo.done),
        id: clickedTodo.id,
      };

      const response = await fetch(`${apiUrl}/todos/${updatedTodo.id}`, {
        method: "PUT",
        headers: {
          "content-type": "application/json",
        },
        body: JSON.stringify(updatedTodo),
      });
      const newUpdatedTodo = await response.json();
      console.log(newUpdatedTodo);

      return newUpdatedTodo;
    },

    async handleDeleteTodo(todoId) {
      await this.deleteTodoFromApi(todoId);
      this.todos = this.todos.filter((todo) => todo.id !== todoId);
    },
  },
  async created() {
    const response = await fetch("http://localhost:4730/todos");
    const apiTodos = await response.json();
    this.todos = apiTodos;
  },
};
</script>
<style>
.taskform {
  width: 100%;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}
</style>
