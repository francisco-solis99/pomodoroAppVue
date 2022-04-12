<!-- JS -->
<script>
// import the components to use
import AddTask from './components/AddTask.vue';
import TasksList from './components/TasksList.vue';
import Timer from './components/Timer.vue';
  export default {
    name: "App",
    components: { AddTask, TasksList, Timer },

    // data
    data() {
      return {
        tasksList: [],
        copyTasks: [],
        taskListRender : false,
        actualTask: {},
      };
    },

    // watchers
    watch: {
      copyTasks(newList){
        newList.length ? this.taskListRender = true : this.taskListRender = false;
      }
    },

    // methods
    methods: {
      addTask(textTask){
        const task = {
          id: this.tasksList.length ? this.tasksList[this.tasksList.length-1].id + 1 : 0,
          taskTitle: textTask,
          inProgress: false,
          finished: false
        };
        this.tasksList.push(task);
        this.saveTasks();

      },

      finishTask(id){
        const index = this.findIndexTask(id);
        if(index === -1) { console.info('This index does not exist'); return;}
        this.tasksList[index].finished =  true;
        this.saveTasks();
      },

      completePomodoro(id){
        const index = this.findIndexTask(id);
        if(index === -1) { console.info('This index does not exist'); return;}
        this.tasksList[index].inProgress =  false;
        this.saveTasks();
      },

      findIndexTask(id){
        const taskIndex =  this.tasksList.findIndex(item => item.id === id);
        return taskIndex;
      },

      startTimer(task){
        task.inProgress = true;
        this.actualTask = task;
      },

       saveTasks(){
        this.copyTasks = [...this.tasksList];
      },
    },

    // hook de created
    created(){
      this.saveTasks();
    }

    /* TODO:
    => 3 dot animation and progress text changed in the button
    => Keep the changes in Local Storage
    => Delete the tasks if we wanted
    => Documentate the app
    */
  };
</script>

<!-- HTML -->
<template>
  <main class="pomodoro__hero">
    <h1 class="pomodoro__title">Pomodoro</h1>
    <Timer v-bind:actualTask = "actualTask" v-on:compled-task="finishTask" v-on:progress-completed="completePomodoro" :timeToWork="0.1" :timeToRest="0.2"></Timer>
    <AddTask v-on:add-task="addTask"></AddTask>
    <section class="pomodoro__tasksList" v-if="taskListRender">
      <TasksList v-bind:tasks="copyTasks" v-on:start-task="startTimer"></TasksList>
    </section>
    <p class="pomodoro__default" v-else>Add something to do 😀</p>
  </main>
</template>

<!-- CSS -->
<style>
@import './assets/base.css';

#app {
  display: grid;
  place-items: center;
  min-height: 100vh;
  background: linear-gradient(-45deg, transparent 10%, var(--color-line) 10% 20%, transparent 20% 80%, var(--color-line) 80% 90%, transparent 90% 100%);
  background-color: var(--color-background);
}

.pomodoro__hero {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 320px;
  max-width: 750px;
  width: 60%;
  /* background-color: aqua; */
}

.pomodoro__title {
  margin-bottom: 10px;
  font-size: 2.8rem;
  font-weight: bold;
  color: white;
  text-shadow: 4px 4px 10px rgba(0, 0, 0, 0.25);
}

.pomodoro__tasksList {
  width: 100%;
  padding: 10px;
  margin-top: 20px;
  background-color: rgba(255, 255, 255, 0.75);
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 10px;
}

.pomodoro__default {
  margin-top: 15px;
  font-weight: 300;
  font-size: 1.2rem;
  color: white;
}

</style>
