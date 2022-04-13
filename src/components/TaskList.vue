<!-- HTML -->
<template>
    <div class="task__container" v-for="task in tasks" v-bind:key="task.id">
       <task-list-item
       v-bind:taskText="task.taskTitle"
       v-bind:taskCompleted="task.finished"
       v-bind:taskInProgress="task.inProgress"
       v-on:start-task="$emit('start-task', task)"
       v-on:delete-task="$emit('delete-task', task)">
       </task-list-item>
    </div>
</template>

<!-- JS -->
<script>
    import TaskListItem from './TaskListItem.vue';
    export default {
        name : 'TaskList',
        emits: ['start-task', 'delete-task'],
        components : {'task-list-item':TaskListItem},
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

  .task__container:hover {
    background-color: rgba(103, 194, 255, 0.2);
  }
</style>
