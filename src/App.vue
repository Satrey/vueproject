<template>
  <v-app>
    <v-main>
      <task-input v-on:signalNewTask="addNewTask" />
      
      <v-card class="mx-auto" max-width="800">
        
        <v-list-item class="pt-6 pb-6 pl-4 pr-4 tasklist d-flex" v-for="(item, index) in list" :key='index'> 
          <v-list-item-header>
            {{ item.finishedAt }} : {{ item.closed }}
            <v-list-item-title class="p-4 mb-2">{{index + 1}} : {{item.task}} : {{ item.published }}</v-list-item-title>
            <v-list-item-subtitle>{{ description }}</v-list-item-subtitle>          
          </v-list-item-header>          
          <v-checkbox hide-details color="red" :checked="!!item.closed" @input="changeTaskStatus(index)"></v-checkbox>
        </v-list-item>
      </v-card>
    </v-main>
  </v-app>
</template>

<script>
import TaskInput from './components/TaskInput.vue'

export default {
  name: 'App',

  components: {
    TaskInput,
  },

  data: () => ({
    list: [],
    description: 'Также как сплочённость команды профессионалов играет важную роль в формировании направлений прогрессивного развития.',
    chackStatus: false,
  }),

  methods: {
    addNewTask(task) {
      console.log('New task recieved from TaskInput component : ', task);
      this.list.push({task: task,
                      published: Date.now(),
                      finishedAt: undefined,
                      closed : false,  
                      });
      console.log(this.list);
    },
    changeTaskStatus(index){
      console.log('Статус : ',index)
      const task = this.list[index];
      if(task.closed){
          task.finishedAt = undefined;
          task.closed = false;
      }
      else {
        task.finishedAt = Date.now();
        task.closed = true;
      }
      
    },
  },
}
</script>

<style scoped>
.tasklist:hover{
  background-color: rgba(194, 194, 194, 0.363);

}
</style>
