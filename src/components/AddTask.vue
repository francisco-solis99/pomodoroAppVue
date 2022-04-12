<!-- JS -->
<script>
  export default {
    name: 'AddTask',
    props: {},
    emits: ['add-task'],
    data() {
      return {
        textTask:''
      };
    },
    methods: {
      sendTask(e){
        if(this.textTask === '') {
          console.warn('Insert a task');
          return;
        }
        this.$emit('add-task', this.textTask);
        e.target.reset(); //clean the form
        this.textTask = '';
      }
    }
  };
</script>

<!-- HTML -->
<template>
  <div class="add-task__container">
    <form action="" class="add-task__form" @submit.prevent="sendTask">
      <input type="text" name="task" placeholder="Add a task..." class="add-task__input" v-model="textTask">
      <button type="submit" class="add-task__button">Add Task</button>
    </form>
  </div>
</template>


<!-- CSS -->
<style scoped>
  .add-task__container {
    background-color: tomato;
    width: 60%;
  }

  .add-task__form {
    display: flex;
    box-shadow: 4px 4px 10px rgba(0, 0, 0, 0.25);
  }

  .add-task__input {
    width: 75%;
    padding: 15px;
    border: 0;
    outline: 0;
    font-size: 1rem;
  }

  .add-task__button {
    min-width: min-content;
    width: 25%;
    border: 0;
    background-color: var( --color-buttons);
    color: white;
    cursor: pointer;
  }

</style>
