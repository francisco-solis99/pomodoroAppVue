<template>
    <div class="timer__container">
        <h1 class="timer__clock">{{displayTimer}}</h1>
        <p class="timer__activity" v-if="!this.actualTask.finished">{{actualTask.taskTitle}}</p>
    </div>
</template>

<script>
    export default {
        name: 'TimerTask',
        // props
        props: ['timeToWork', 'timeToRest', 'actualTask'],

        // emits
        emits: ['compled-task'],

        // data
        data() {
          return {
            workingTimeInSeconds: this.timeToWork * 60,
            countdown: null,
          };
        },

        computed:{
          displayTimer(){
            const minutes = Math.floor(this.workingTimeInSeconds / 60);
            const remindSeconds = this.workingTimeInSeconds % 60;
            const display = `${minutes}:${remindSeconds < 10 ? '0' + remindSeconds : remindSeconds}`;
            document.title = `${this.actualTask.taskTitle ?? 'Pomodoro'} - ${display}`;
            return display;
          }
        },

        // methods
        methods: {
          startTimer(){
            if(this.actualTask.finished) { console.log('ya esta completa la task'); return;}
            clearInterval(this.countdown);

            const now = Date.now();
            const then = now + (this.workingTimeInSeconds * 1000);


            this.countdown = setInterval(() => {
              const seconds = Math.round((then - Date.now()) / 1000);
              if(seconds < 0){
                clearInterval(this.countdown);
                this.workingTimeInSeconds = this.timeToWork * 60,
                document.title = `Pomodoro - ${this.displayTimer}`;
                this.$emit('compled-task', this.actualTask);
                return;
              }
              this.workingTimeInSeconds = seconds;
            }, 1000);
          },
        },

        // Vue hooks component
        updated(){
          this.startTimer();
        },



    };
</script>

<style scoped>
</style>
