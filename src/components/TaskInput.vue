<template>
  <div>
    <v-card class="mx-auto mt-10 mb-10 pb-5" max-width="800">
      <v-card-title class="p-3">
          <h2>Task : {{ task }}</h2>
      </v-card-title>
      <!-- <div class="input_row d-flex justify-space-between"> -->
        <v-col >
          <v-text-field class="mb-4" hide-details v-model="task" label="Contained" variant="contained" clearable clear-icon="mdi-delete">
          </v-text-field>
          <v-text-field hide-details v-model="description" label="Contained" variant="contained" clearable clear-icon="mdi-delete">
          </v-text-field>
        </v-col>
        <v-container class="d-flex justify-end me-1">
          <v-btn color="primary" @click="addTask">
            Добавить
          </v-btn>
        </v-container>
      <!-- </div> -->
    </v-card>
  </div>
</template>
<script>
export default {
  name: 'TaskInput',
  data() {
    return {
      task: '',
      description: '',
    }
  },
  methods: {
    addTask(){
      this.$emit('signalNewTask', this.task, this.description);
      console.log(this.task, 'added');
      console.log(this.description, 'added');
      this.task = '';
      this.description = '';

    },
  },

}
</script>
<style lang="css">
  .input_row {
    display: flex;
  }
</style>