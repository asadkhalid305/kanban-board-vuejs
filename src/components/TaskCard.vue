<template>
  <v-card class="mx-auto elevation-2 text-start" max-width="384" outline>
    <v-card-title class="text-capitalize">{{ task.title }}</v-card-title>
    <v-card-text>
      <div class="mb-4">{{ task.description }}</div>
      <div class="d-flex">
        <v-chip v-show="task.date">{{ task.date }}</v-chip>
        <v-chip v-show="task.priority" class="ml-1 text-capitalize">{{ task.priority }}</v-chip>
        <v-chip v-show="task.tag" class="ml-1 text-capitalize">{{task.tag}}</v-chip>
        <v-spacer></v-spacer>
        <v-btn icon @click="updateTask(task)">
          <v-icon>mdi-circle-edit-outline</v-icon>
        </v-btn>
        <v-btn icon @click="removeTask(task)">
          <v-icon>mdi-delete</v-icon>
        </v-btn>
      </div>
    </v-card-text>
  </v-card>
</template>
<script>
export default {
  components: {},
  props: {
    task: {
      type: Object,
      default: () => ({})
    }
  },
  methods: {
    removeTask({ id }) {
      this.$emit("onRemove", { id });
    },
    updateTask(task) {
      this.$emit("onUpdate", task);
    }
  }
};
</script>

<style lang="scss" scoped>
.v-card {
  .v-card__title,
  .v-card__description {
    word-break: break-word;
  }
}
</style>
