<!-- HTML -->
<template>
    <div class="task__container" v-for="task in tasks" v-bind:key="task.id">
       <Task v-bind:taskText="task.taskTitle"  v-bind:taskCompleted="task.finished" v-bind:taskInProgress="task.inProgress" v-on:start-task="$emit('start-task', task)" v-on:delete-task="$emit('delete-task', task)"></Task>
    </div>
</template>

<!-- JS -->
<script>
    import Task from './Task.vue';
    export default {
        name : 'TasksList',
        emits: ['start-task', 'delete-task'],
        components : {Task},
        props: ['tasks'],
    };
</script>


<!-- CSS -->
<style scoped>
  .task__container {
    display: flex;
    align-items: center;
    width: 100%;
    position: relative;
  }
</style>
