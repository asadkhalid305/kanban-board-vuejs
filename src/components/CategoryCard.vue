<template>
  <v-card>
    <v-virtual-scroll :items="items" :item-height="50" height="300">
      <template v-slot:default="{ item }">
        <v-card class="mx-auto" max-width="344">
          <v-card-text>
            <p class="text-h4 text--primary">{{ item.title }}</p>
            <div class="text--primary">
              {{ item.description }}
            </div>
            <p>{{ item.date }}</p>
            <v-spacer></v-spacer>
            <p>{{ item.priority }}</p>
          </v-card-text>
          
        </v-card>
      </template>
    </v-virtual-scroll>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      items: [
          {
              title: 'task1',
              description: 'task 1',
              date: '02/08/21',
              priority: 'high'

          },
          {
              title: 'task2',
              description: 'task 3',
              date: '02/08/21',
              priority: 'high'

          },
          {
              title: 'task3',
              description: 'task 2',
              date: '02/08/21',
              priority: 'high'

          }
      ]
    };
  }
};
</script>

<style>
</style>