<template>
  <v-card class="mx-auto elevation-2 text-start" max-width="384" outline>
    <v-card-title class="text-capitalize">{{ task.title }}</v-card-title>
    <v-card-text>
      <div class="mb-4">{{ task.description }}</div>
      <div class="d-flex">
        <p>{{ task.date }}</p>
        <v-spacer></v-spacer>
        <p>{{task.type}}</p>
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
  }
};
</script>

<style lang="scss" scoped>
.v-card {
  .v-card__title, .v-card__description {
    word-break: break-word;
  }
}
</style>
