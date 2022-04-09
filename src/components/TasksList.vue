<template>
    <div class="task__container" v-for="task in tasks" v-bind:key="task.id">
       <Task v-bind:taskText="task.taskTitle" v-bind:taskCompleted="task.finished" v-on:start-task="$emit('start-task', task)"></Task>
    </div>
</template>
<script>
    import Task from './Task.vue';
    export default {
        name : 'TasksList',
        emits: ['start-task'],
        components : {Task},
        props: ['tasks'],
    };
</script>

<style scoped>
  .task__container {
    display: flex;
    align-items: center;
  }
</style>
