<template>
  <v-app>
    <v-app-bar app color="#f5f5f5" class dark>
      <v-row>
        <v-col class="text-center">
          <div class="text-h5 font-weight-bold text-uppercase text--primary">Kanban Board VueJs</div>
        </v-col>
      </v-row>
    </v-app-bar>

    <v-main>
      <v-container fluid>
        <v-row class="category">
          <!-- iterate each category card -->
          <v-col v-for="column in columns" :key="column.title" cols="3">
            <!-- single category card -->
            <v-card elevation="4">
              <!-- category title -->
              <v-card-title>
                  <v-badge
                    color="black"
                    inline
                    left
                    :content="`${column.tasks.length}`"
                  >{{column.title}}</v-badge>
                  <v-spacer></v-spacer>
                  <v-btn v-if="column.key === 'backlog'" @click="openTaskModal()" icon small color="black">
                    <v-icon dark>mdi-plus</v-icon>
                  </v-btn>
              </v-card-title>
              <!-- category body -->
              <v-card-text class="text-center pt-2">
                <div class="pb-2">
                  <!-- Draggable component comes from vuedraggable. It provides drag & drop functionality -->
                  <draggable :list="column.tasks" :animation="200" group="tasks">
                    <!-- Each element from here will be draggable and animated. Note :key is very important here to be unique both for draggable and animations to be smooth & consistent. -->
                    <!-- iterate each task -->
                    <template v-if="column.tasks && column.tasks.length">
                      <task-card
                        class="mt-3 cursor-move"
                        v-for="(task) in column.tasks"
                        :key="task.id"
                        :task="task"
                        @onRemove="removeTask"
                        @onUpdate="openTaskModal"
                      />
                    </template>
                    <div v-else>No task available</div>
                  </draggable>
                </div>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
      <task-card-form
        v-if="taskModalState"
        :modal-state.sync="taskModalState"
        :mode="taskModalMode"
        :task="taskDetails"
        @onSave="createOrUpdateTask"
      />
    </v-main>
  </v-app>
</template>

<script>
// packages
import draggable from "vuedraggable";

// components
import TaskCard from "./components/TaskCard.vue";
import TaskCardForm from "./components/TaskCardForm.vue";

// services
import { constantsService } from "./services/constantsService";

export default {
  name: "App",
  components: {
    TaskCard,
    draggable,
    TaskCardForm
  },
  data() {
    return {
      taskModalState: false,
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
              tag: "feature",
              priority: "highest"
            },
            {
              id: 2,
              title: "Provide documentation on integrations",
              description: "Add discount code to checkout page",
              date: "Sep 12",
              priority: "highest"
            },
            {
              id: 3,
              title: "Design shopping cart dropdown",
              description: "Add discount code to checkout page",
              date: "Sep 9",
              tag: "Design",
              priority: "highest"
            },
            {
              id: 4,
              title: "Add discount code to checkout page",
              description: "Add discount code to checkout page",
              date: "Sep 14",
              tag: "feature"
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
              tag: "feature"
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
              tag: "feature"
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
              tag: "feature"
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
              tag: "feature"
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
              tag: "feature"
            }
          ]
        }
      ],
      taskModalMode: constantsService.taskModalMode.create,
      taskDetails: {}
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
    createOrUpdateTask(formData) {
      let newTask = {};
      if (this.taskModalMode === constantsService.taskModalMode.create) {
        // eslint-disable-next-line no-unused-vars
        const { id, ...payload } = formData;
        newTask = { ...payload };
        newTask.id = Math.random();
      } else {
        newTask = formData;
      }
      // hit POST API
      // remeove this part
      this.columns.forEach(column => {
        if (column.key !== "backlog") {
          return;
        }
        if (this.taskModalMode === constantsService.taskModalMode.create) {
          column.tasks.unshift(newTask);
          return;
        }
        column.tasks = column.tasks.map(task => {
          if (task.id !== newTask.id) {
            return task;
          }
          return newTask;
        });
      });
    },
    removeTask({ id }) {
      // hit POST API
      // remove this part
      this.columns.forEach(column => {
        column.tasks = column.tasks.filter(task => task.id !== id);
      });
    },
    openTaskModal(task = null) {
      if (!task) {
        this.taskModalMode = constantsService.taskModalMode.create;
      } else {
        this.taskDetails = task;
        this.taskModalMode = constantsService.taskModalMode.edit;
      }
      this.taskModalState = true;
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
        padding: 14px;
        font-size: 18px;
        background-color: #ffffff;
      }
      .v-card__text {
        overflow-y: auto;
        height: 85vh;
        background-color: #f5f5f5;
      }
    }
  }
}
</style>
