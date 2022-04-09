<script>

    export default {
        name: 'PomodoroTask',
        props: ['taskText', 'taskCompleted'],
        data() {
            return {
                finished: this.taskCompleted
            };
        },
        emits: ['start-task'],
        computed: {
            btnStyle(){
                return this.taskCompleted ? ['completed__btn', 'task__button '] : ['task__button', 'incople__task'];
            },

            pStyle(){
                return this.taskCompleted ? ['task__text ', 'completed__task'] : ['task__text'];
            },
        },
        watch:{
            finished(){
                // this coulb be better
                this.finished ? this.$ref.buttonStart.disabled = true : this.$ref.buttonStart.disabled = false;
            }
        }
    };
</script>

<template>
      <button :class="btnStyle" @click="$emit('start-task', taskText)" ref="buttonStart">Start</button>
      <p :class="pStyle">{{taskText}}</p>
</template>

<style scoped>
    .task__button {
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

    .incople__task {
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
