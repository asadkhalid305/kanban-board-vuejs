<template>
  <v-row justify="center">
    <v-dialog v-model="modalState" persistent max-width="600px">
      <v-form ref="form" v-model="isFormValid">
        <v-card>
          <v-card-title>
            <span class="text-h5">New Task</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12">
                  <v-text-field
                    v-model="formData.title"
                    :rules="[rules.required]"
                    label="Title*"
                    required
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    v-model="formData.description"
                    :rules="[rules.required]"
                    label="Description*"
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-autocomplete
                    v-model="formData.priority"
                    :items="priorityOptions"
                    :rules="[rules.required]"
                    label="Priority*"
                    hint="Default priority is Low"
                  ></v-autocomplete>
                </v-col>
                <v-col cols="12">
                  <v-autocomplete v-model="formData.tag" :items="tagsOptions" label="Tag"></v-autocomplete>
                </v-col>
              </v-row>
            </v-container>
            <small>* indicates required field</small>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" text @click="$emit('update:modalState', !modalState)">Close</v-btn>
            <v-btn color="blue darken-1" :disabled="!isFormValid" text @click="saveNewCard">Save</v-btn>
          </v-card-actions>
        </v-card>
      </v-form>
    </v-dialog>
  </v-row>
</template>

<script>
import _cloneDeep from "lodash/cloneDeep";

import { constantsService } from "../services/constantsService";

export default {
  props: {
    modalState: Boolean,
    mode: Number,
    task: {
      type: Object,
      default: () => ({})
    }
  },
  data: () => ({
    priorityOptions: [
      { text: "Highest", value: "highest" },
      { text: "High", value: "high" },
      { text: "Medium", value: "medium" },
      { text: "Low", value: "low" }
    ],
    tagsOptions: [
      { text: "Feature", value: "feature" },
      { text: "Task", value: "task" },
      { text: "Bug", value: "bug" }
    ],
    isFormValid: false,
    formData: {
      title: "",
      description: "",
      priority: "",
      tag: ""
    },
    rules: {
      required: value => !!value || "Required"
    }
  }),
  created() {
    if (!this.mode === constantsService.taskModalMode.edit) {
      return;
    }
    this.formData = _cloneDeep(this.task);
  },
  methods: {
    saveNewCard() {
      this.$emit("onSave", this.formData);
      this.$emit("update:modalState", !this.modalState);
    }
  }
};
</script>