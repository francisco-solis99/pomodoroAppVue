<!-- JS -->
<script>

    export default {
        name: 'PomodoroTask',
        props: ['taskText', 'taskCompleted', 'taskInProgress'],
        emits: ['start-task'],

        computed: {
            btnStyle(){
                return this.taskCompleted ? ['completed__btn', 'task__button '] : ['task__button', 'incompled__task'];
            },

            pStyle(){
                return this.taskCompleted ? ['task__text', 'completed__task'] : ['task__text'];
            },
        },
        watch:{
            taskCompleted(newValue){
                if(newValue){
                   this.$refs.buttonStart.disabled = true;
                   this.$refs.buttonStart.textContent = 'Done';
                }
            },

            taskInProgress(newValue) {
                if(newValue) this.$refs.buttonStart.textContent = 'In Progress';
            }

        }
    };
</script>

<!-- HTML -->
<template>
      <button :class="btnStyle" @click="$emit('start-task', taskText)" ref="buttonStart">Start</button>
      <p :class="pStyle">{{taskText}}</p>
</template>


<!-- CSS -->
<style scoped>
    .task__button {
        min-width: min-content;
        width: 30%;
        max-width: 80px;
        padding: 10px;
        border: none;
    }

    .task__text {
      font-size: 0.9rem;
      margin-left:5px;
      word-break: break-word;
    }

    .completed__task {
        text-decoration: line-through;
    }

    .incompled__task {
        color: white;
        background-color: var(--color-buttons);
        cursor: pointer;

    }
    .completed__btn {
        color: black;
        background-color: rgb(170, 170, 170);
        cursor: not-allowed;
    }


</style>
