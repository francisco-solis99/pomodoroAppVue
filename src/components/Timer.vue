<template>
    <div class="timer__container">
        <h1 class="timer__clock">{{displayTimer}}</h1>
        <p class="timer__activity" v-if="!this.actualTask.finished">{{actualTask.taskTitle}}</p>
        <p class="timer__activity" v-else-if="this.actualTask.inProgress">Break Time</p>
    </div>
</template>

<script>
    export default {
        name: 'TimerTask',
        // props
        props: ['timeToWork', 'timeToRest', 'actualTask'],

        // emits
        emits: ['compled-task', 'progress-completed'],

        // data
        data() {
          return {
            timer: this.timeToWork * 60,
            countdown: null,
          };
        },

        computed:{
          displayTimer(){
            const minutes = Math.floor(this.timer / 60);
            const remindSeconds = this.timer % 60;
            const display = `${minutes}:${remindSeconds < 10 ? '0' + remindSeconds : remindSeconds}`;
            document.title = `${this.actualTask.taskTitle ?? 'Pomodoro'} - ${display}`;
            return display;
          }
        },

        // methods
        methods: {
          startPomodoro(){
            if(!this.actualTask.inProgress) return;
            clearInterval(this.countdown);
            const now = Date.now();
            const then = now + (this.timer * 1000);

            this.countdown = setInterval(() => {
              const seconds = Math.round((then - Date.now()) / 1000);
              if(seconds < 0){
                clearInterval(this.countdown);
                document.title = `Pomodoro - ${this.displayTimer}`;
                if(this.actualTask.finished){
                  this.timer = this.timeToWork * 60;
                  this.$emit('progress-completed', this.actualTask.id);
                }
                else {
                  this.timer = this.timeToRest * 60;
                  this.$emit('compled-task', this.actualTask.id); // emit for cross the task
                }
                return;
              }
              this.timer = seconds;
            }, 1000);

          },

        },

        // Vue hooks component
        updated(){
          this.startPomodoro();
        },
    };
</script>

<style scoped>
</style>
