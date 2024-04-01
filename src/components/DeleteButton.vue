<template>
  <button @click="deleteTodo" v-if="hasDoneTodos">
    Remove
    {{ console.log(items) }}
  </button>
</template>

<script>
export default {
  name: "DeleteButton",
  props: {
    items: {
      type: Array,
      default() {
        return [];
      },
    },
  },
  computed: {
    hasDoneTodos() {
      return this.items.some((item) => item.done);
    },
  },
  methods: {
    deleteTodo() {
      if (confirm("Do you really want to delete all completed tasks?")) {
        const doneTodo = this.items
          .filter((item) => item.done)
          .map((item) => item.id);
        this.$emit("deleteDoneTodo", doneTodo);

        if (doneTodo.length === 0) {
          alert("No completed tasks to delete");
          return;
        }
      }
    },
  },
};
</script>
