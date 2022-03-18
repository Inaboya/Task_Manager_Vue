<template>
  <header>
    <h1>{{ title }}</h1>
    <HeaderButton
      @btn-click="$emit('toggle-add-task')"
      :text="!showAddTask ? 'Add Task' : 'Close'"
      :color="!showAddTask ? 'green' : 'red'"
    />
  </header>
</template>

<script>
import HeaderButton from "./HeaderButton.vue";

export default {
  name: "HeaderComponent",
  props: {
    title: {
      type: String,
    },
    showAddTask: {
      type: Boolean,
    },
  },
  components: {
    HeaderButton,
  },
};
</script>
