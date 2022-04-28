<template>
  <div>
    <v-card class="mx-auto mt-10 mb-10 pb-5" max-width="800">
      <v-card-title class="p-3">
        <h2>Task : {{ task }}</h2>
      </v-card-title>
      <div class="input_row">
        <v-col>
          <v-text-field hide-details v-model="task" label="Contained" variant="contained" clearable clear-icon="mdi-delete">
          </v-text-field>
        </v-col>
        <v-card-actions>
          <v-btn variant="text" color="teal-accent-4" @click="addTask">
            Добавить
          </v-btn>
        </v-card-actions>
      </div>
    </v-card>
  </div>
</template>
<script>
export default {
  name: 'TaskInput',
  data() {
    return {
      task: '',
    }
  },
  methods: {
    addTask(){
      this.$emit('signalNewTask', this.task);
      console.log(this.task, 'added');
      this.task = '';
    },
  },

}
</script>
<style lang="css">
  .input_row {
    display: flex;
  }
</style>