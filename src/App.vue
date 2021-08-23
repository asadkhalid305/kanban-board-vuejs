<template>
  <v-app>
    <v-app-bar color="black" dark>
      <v-row>
        <v-col class="text-center">
          <h1>Kanban Board VueJs</h1>
        </v-col>
      </v-row>
    </v-app-bar>

    <v-main>
      <v-container fluid>
        <v-row class="category">
          <!-- iterate each category card -->
          <v-col v-for="column in columns" :key="column.title" cols="3">
            <!-- single category card -->
            <v-card elevation="3" outline tile>
              <!-- category title -->
              <v-card-title
                :class="[{ 'justify-center': column.key !== 'backlog' },column.color]"
                :style="`background-color: ${column.color}`"
              >
                <v-row v-if="column.key === 'backlog'" no-gutters>
                  <v-col cols="5">
                    <v-btn @click="newCardModal = true" outlined fab x-small color="black">
                      <v-icon dark>mdi-plus</v-icon>
                    </v-btn>
                  </v-col>
                  <v-col>{{column.title}}</v-col>
                </v-row>
                <div v-else>{{column.title}}</div>
              </v-card-title>
              <!-- category body -->
              <v-card-text class="text-center pt-2">
                <div class="category-card-body-scroll pb-2">
                  <!-- Draggable component comes from vuedraggable. It provides drag & drop functionality -->
                  <draggable :list="column.tasks" :animation="200" group="tasks">
                    <!-- Each element from here will be draggable and animated. Note :key is very important here to be unique both for draggable and animations to be smooth & consistent. -->
                    <!-- iterate each task -->
                    <template v-if="column.tasks && column.tasks.length">
                      <task-card
                        v-for="(task) in column.tasks"
                        :key="task.id"
                        :task="task"
                        class="mt-3 cursor-move"
                      ></task-card>
                    </template>
                    <div v-else>No task available</div>
                  </draggable>
                </div>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
      <create-task-card
        v-if="newCardModal"
        :modal-state.sync="newCardModal"
        @onSave="createNewCard"
      />
    </v-main>
  </v-app>
</template>

<script>
import draggable from "vuedraggable";
import TaskCard from "./components/TaskCard.vue";
import CreateTaskCard from "./components/CreateTaskCard.vue";

export default {
  name: "App",
  components: {
    TaskCard,
    draggable,
    CreateTaskCard
  },
  data() {
    return {
      newCardModal: false,
      columns: [
        {
          title: "Backlog",
          key: "backlog",
          color: "#65AFFF",
          tasks: [
            {
              id: 1,
              title: "Add discount code to checkout page",
              description:
                "Add discount code to checkout page discount code to checkout page discount code to checkout page discount code to checkout page discount code to checkout page",
              date: "Sep 14",
              tag: "Feature Request"
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
              tag: "Design"
            },
            {
              id: 4,
              title: "Add discount code to checkout page",
              description: "Add discount code to checkout page",
              date: "Sep 14",
              tag: "Feature Request"
            },
            {
              id: 5,
              title: "Test checkout flow",
              description: "Add discount code to checkout page",
              date: "Sep 15",
              tag: "QA"
            },
            {
              id: 100,
              title: "Test checkout flow",
              description: "Add discount code to checkout page",
              date: "Sep 15",
              tag: "QA"
            }
          ]
        },
        {
          title: "In Progress",
          color: "#53F4FF",
          tasks: [
            {
              id: 6,
              title: "Design shopping cart dropdown",
              date: "Sep 9",
              tag: "Design"
            },
            {
              id: 7,
              title: "Add discount code to checkout page",
              date: "Sep 14",
              tag: "Feature Request"
            },
            {
              id: 8,
              title: "Provide documentation on integrations",
              date: "Sep 12",
              tag: "Backend"
            }
          ]
        },
        {
          title: "Review",
          color: "#B288C0",
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
              tag: "Design"
            },
            {
              id: 11,
              title: "Add discount code to checkout page",
              date: "Sep 14",
              tag: "Feature Request"
            },
            {
              id: 12,
              title: "Design shopping cart dropdown",
              date: "Sep 9",
              tag: "Design"
            },
            {
              id: 13,
              title: "Add discount code to checkout page",
              date: "Sep 14",
              tag: "Feature Request"
            }
          ]
        },
        {
          title: "Done",
          color: "#51E181",
          tasks: [
            {
              id: 14,
              title: "Add discount code to checkout page",
              date: "Sep 14",
              tag: "Feature Request"
            },
            {
              id: 15,
              title: "Design shopping cart dropdown",
              date: "Sep 9",
              tag: "Design"
            },
            {
              id: 16,
              title: "Add discount code to checkout page",
              date: "Sep 14",
              tag: "Feature Request"
            }
          ]
        }
      ]
    };
  },
  computed: {
    date() {
      let dateObj = new Date();
      let month = dateObj.getUTCMonth() + 1; //months from 1-12
      let day = dateObj.getUTCDate();
      let year = dateObj.getUTCFullYear();

      return year + "/" + month + "/" + day;
    }
  },
  methods: {
    createNewCard(formData) {
      const { title, description, priority, tag } = formData;
      const newTask = {
        id: Math.random(),
        title,
        description,
        priority,
        tag,
        date: this.date
      };
      this.columns.forEach(column => {
        if (column.key === "backlog") {
          column.tasks.unshift(newTask);
        }
      });
      // hit POST API
    }
  }
};
</script>

<style lang='scss' scoped>
.category {
  flex-wrap: nowrap;
  overflow-x: auto;

  .col {
    .v-card {
      .v-card__title {
        padding: 12px;
        font-size: 18px;
      }
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
