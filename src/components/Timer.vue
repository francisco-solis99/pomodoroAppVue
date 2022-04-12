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
            // timer to be updated adn  displayed using a computend function
            timer: this.timeToWork * 60,
            // countdown to start and stop isging interval function
            countdown: null,
          };
        },

        // computed
        computed:{
          displayTimer(){
            // formatng the seconds time in minutes||seconds
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
            // clean the interval at the beginning
            clearInterval(this.countdown);
            // get the current now in ms vs the future moment in ms when the target time has completed
            const now = Date.now();
            const then = now + (this.timer * 1000);

            // begin the interlval
            this.countdown = setInterval(() => {
              const seconds = Math.round((then - Date.now()) / 1000);
              if(seconds < 0){
                clearInterval(this.countdown);
                document.title = `Pomodoro - ${this.displayTimer}`;
                // in case the task is finished the timer get back to it initial state and make a emit, the first time this is not happened
                if(this.actualTask.finished){
                  this.timer = this.timeToWork * 60;
                  this.$emit('progress-completed', this.actualTask.id);
                }
                //the first time this happened cause the task does not finished yet, so the time to rest begin and right here we shota emit to complete the task complete state
                else {
                  this.timer = this.timeToRest * 60;
                  this.$emit('compled-task', this.actualTask.id); // emit for cross the task
                }
                return;
              }
              // in case the seconds are bigger than 0, the the timer is updated every single second
              this.timer = seconds;
            }, 1000);

          },

        },

        // Vue hooks component
        updated(){
          // start the timer as long as the task progress property is true
          this.startPomodoro();
        },
    };
</script>

<style scoped>
</style>
