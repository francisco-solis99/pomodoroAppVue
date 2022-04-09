<template>
    <div class="timer__container">
        <h1 class="timer__clock">{{displayTimer}}</h1>
        <p class="timer__activity">{{actualTask.taskTitle}}</p>
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
            completed: false,
          };
        },

        computed:{
          displayTimer(){
            const minutes = Math.floor(this.workingTimeInSeconds / 60);
            const remindSeconds = this.workingTimeInSeconds % 60;
            const display = `${minutes}:${remindSeconds < 10 ? '0' + remindSeconds : remindSeconds}`;
            document.title = display;
            return display;
          }
        },

        // methods
        methods: {
          startTimer(){
            clearInterval(this.countdown);

            const now = Date.now();
            const then = now + (this.workingTimeInSeconds * 1000);


            this.countdown = setInterval(() => {
              console.log('en el interval');
              const seconds = Math.round((then - Date.now()) / 1000);
              if(seconds < 0){
                clearInterval(this.countdown);
                this.$emit('compled-task', this.actualTask);
                console.log('despues del emit');
                return;
              }
              this.workingTimeInSeconds = seconds;
            }, 1000);
          },
        },

        updated(){
          this.startTimer();
        },



    };
</script>

<style scoped>
</style>
