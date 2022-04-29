<template>
  <v-app>
    <v-main>
      <task-input v-on:signalNewTask="addNewTask" />
      
      <v-card class="mx-auto" max-width="800">
        <v-container class="d-flex justify-start  task_title" v-if="list.length > 0">
          <v-checkbox class="" hide-details color="green" label="Hide Done" v-model="hideDone"></v-checkbox>
          <v-checkbox class="" hide-details color="green" label="Reverse list" v-model="reverse"></v-checkbox>
          <v-checkbox class="" hide-details color="green" label="Sort by ID" v-model="sortById"></v-checkbox>
        </v-container>
        <v-list-item class="pt-6 pb-6 pl-4 pr-4 tasklist d-flex justify-space-between mb-6" v-for="(item, index) in displayList" :key='index'> 
          <v-list-item-header>
            <v-list-item-title class="p-4 mb-2">{{item.id}} : {{item.task}} : {{ formatDate(item.published) }} </v-list-item-title>
            <v-list-item-subtitle>{{ item.description }}</v-list-item-subtitle>          
            <v-list-item-subtitle v-if="item.closed">Завершена : {{ formatDate(item.finishedAt) }}</v-list-item-subtitle>
          </v-list-item-header>          
          <v-checkbox class="justify-end mb-6" hide-details color="red" v-model="item.closed" @input="changeTaskStatus(item.id)"></v-checkbox>
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
    hideDone: false,
    reverse : false,
    sortById: true, 
  }),

  computed: {
    baseList(){
      return [...this.list]
          .map((t, index) => ({
            ...t,
            id: index + 1
          }));
    },

    filteredList(){
      return this.hideDone
          ?  [... this.baseList]
          .filter(t => !t.finishedAt)
          : this.baseList;
    },

    sortedList(){
      return [...this.filteredList]
          .sort((a,b) => (this.sortById ? b.id - a.id : (a.finishedAt || 0) - (b.finishedAt || 0)));
    },

    displayList(){
      const taskList = [...this.sortedList];
      return this.reverse ? taskList.reverse() : taskList;
    },
  },

  methods: {
    addNewTask(task, description) {
      console.log('New task recieved from TaskInput component : ', task);
      this.list.push({task: task,
                      description: description,
                      published: Date.now(),
                      finishedAt: undefined,
                      closed : false,  
                      });
      console.log(this.list);
    },
    
    changeTaskStatus(index){
      
      const task = this.list[index - 1];
      if(task.closed){
          task.finishedAt = undefined;
          task.closed = false;
          console.log('Статус : ',task.closed)
      }
      else {
        task.finishedAt = Date.now();
        task.closed = true;
        console.log(task.closed)
        console.log(this.list)
      }  
    },

    formatDate(value){
      if(!value) return '';
      if(typeof value !== 'number') return value;

      const browserLocale = navigator.languages && navigator.languages.length
            ? navigator.languages[0]
            : navigator.language;
      const intDateTime   = new Intl.DateTimeFormat(browserLocale,
      {
        year: 'numeric',
        month: 'numeric',
        day: 'numeric',
        hour: 'numeric',
        minute: 'numeric',
        second: 'numeric',
      });
      return intDateTime.format(new Date(value));
    },
  },
}
</script>

<style scoped>
.tasklist:hover{
  background-color: rgba(194, 194, 194, 0.363);

}

.task_title{
  background-color: rgb(102, 0, 219);
  border-top-left-radius: 4px;
  border-top-right-radius: 4px;
  color: white;
}
</style>
