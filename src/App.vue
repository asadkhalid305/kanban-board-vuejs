<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <h1>Vue Draggable</h1>
      </div>
    </v-app-bar>

    <v-main>
      <v-container fluid>
        <v-row class="category">
          <v-col v-for="column in columns" :key="column.title" cols="3">
            <v-card elevation="3" outline tile>
              <v-card-title :class="column.color" class="justify-center">{{column.title}}</v-card-title>
              <v-card-text class="text-center pt-2">
                <div class="category-card-body-scroll pb-2">
                  <!-- Draggable component comes from vuedraggable. It provides drag & drop functionality -->
                  <draggable
                    :list="column.tasks"
                    :animation="200"
                    group="tasks"
                  >
                    <!-- Each element from here will be draggable and animated. Note :key is very important here to be unique both for draggable and animations to be smooth & consistent. -->
                    <task-card
                      v-for="(task) in column.tasks"
                      :key="task.id"
                      :task="task"
                      class="mt-3 cursor-move"
                    ></task-card>
                  </draggable>
                </div>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import draggable from "vuedraggable";
import TaskCard from "./components/TaskCard.vue";

export default {
  name: "App",
  components: {
    TaskCard,
    draggable
  },
  data() {
    return {
      columns: [
        {
          title: "Backlog",
          color: "red",
          tasks: [
            {
              id: 1,
              title: "Add discount code to checkout page",
              description: "Add discount code to checkout page discount code to checkout page discount code to checkout page discount code to checkout page discount code to checkout page",
              date: "Sep 14",
              type: "Feature Request"
            },
            {
              id: 2,
              title: "Provide documentation on integrations",
              description: "Add discount code to checkout page",
              date: "Sep 12"
            },
            {
              id: 3,
              title: "Design shopping cart dropdown",
              description: "Add discount code to checkout page",
              date: "Sep 9",
              type: "Design"
            },
            {
              id: 4,
              title: "Add discount code to checkout page",
              description: "Add discount code to checkout page",
              date: "Sep 14",
              type: "Feature Request"
            },
            {
              id: 5,
              title: "Test checkout flow",
              description: "Add discount code to checkout page",
              date: "Sep 15",
              type: "QA"
            },
            {
              id: 100,
              title: "Test checkout flow",
              description: "Add discount code to checkout page",
              date: "Sep 15",
              type: "QA"
            }
          ]
        },
        {
          title: "In Progress",
          color: "green",
          tasks: [
            {
              id: 6,
              title: "Design shopping cart dropdown",
              date: "Sep 9",
              type: "Design"
            },
            {
              id: 7,
              title: "Add discount code to checkout page",
              date: "Sep 14",
              type: "Feature Request"
            },
            {
              id: 8,
              title: "Provide documentation on integrations",
              date: "Sep 12",
              type: "Backend"
            }
          ]
        },
        {
          title: "Review",
          color: "blue",
          tasks: [
            {
              id: 9,
              title: "Provide documentation on integrations",
              date: "Sep 12"
            },
            {
              id: 10,
              title: "Design shopping cart dropdown",
              date: "Sep 9",
              type: "Design"
            },
            {
              id: 11,
              title: "Add discount code to checkout page",
              date: "Sep 14",
              type: "Feature Request"
            },
            {
              id: 12,
              title: "Design shopping cart dropdown",
              date: "Sep 9",
              type: "Design"
            },
            {
              id: 13,
              title: "Add discount code to checkout page",
              date: "Sep 14",
              type: "Feature Request"
            }
          ]
        },
        {
          title: "Done",
          color: "yellow",
          tasks: [
            {
              id: 14,
              title: "Add discount code to checkout page",
              date: "Sep 14",
              type: "Feature Request"
            },
            {
              id: 15,
              title: "Design shopping cart dropdown",
              date: "Sep 9",
              type: "Design"
            },
            {
              id: 16,
              title: "Add discount code to checkout page",
              date: "Sep 14",
              type: "Feature Request"
            }
          ]
        },
        {
          title: "Released",
          color: "indigo",
          tasks: [
            {
              id: 17,
              title: "Add discount code to checkout page",
              date: "Sep 14",
              type: "Feature Request"
            },
            {
              id: 18,
              title: "Design shopping cart dropdown",
              date: "Sep 9",
              type: "Design"
            },
            {
              id: 19,
              title: "Add discount code to checkout page",
              date: "Sep 14",
              type: "Feature Request"
            }
          ]
        }
      ]
    };
  }
};
</script>

<style lang='scss' scoped>
.category {
  flex-wrap: nowrap;
  overflow-x: auto;

  .col {
    .v-card {
      .v-card__text {
        .category-card-body-scroll {
          overflow-y: auto;
          height: 82vh;
        }
      }
    }
  }
}
</style>
