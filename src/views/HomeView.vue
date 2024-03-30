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
      <header></header>
      <main>
        <article class="todo-article" id="app">
          <h1>My Todo-App with Vue</h1>
          <h2 class="claim">Getting things done…</h2>
          <TaskForm />
          <TaskList />
        </article>
      </main>
    </body>
  </html>
</template>

<script>
import TaskForm from "@/components/TaskForm.vue";
import TaskList from "@/components/TaskList.vue";

export default {
  name: "HomeView",
  components: {
    TaskForm,
    TaskList,
  },
};
</script>
