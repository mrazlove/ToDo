<template>
  <div class="completed__task">
    <li :class="{ completed: task.completed }" class="task">
      <span @click="toggleTaskCompletion">{{ task.text }}</span>
      <button class="button__edit" @click="editTask">Edit</button>
      <button class="button__delete" @click="$emit('delete-task', index)">Delete</button>
      <button class="button__complete" @click="completeTask">Complete</button>
    </li>
  </div>
</template>

<script>
export default {
  props: ['task', 'index'],
  emits: ['delete-task', 'update-tasks', 'complete-task'],
  methods: {
    toggleTaskCompletion() {
      this.task.completed = !this.task.completed;
      this.$emit('update-tasks');
    },
    editTask() {
      const newText = prompt("Edit task:", this.task.text);
      if (newText !== null && newText.trim() !== "") {
        this.task.text = newText.trim();
        this.$emit('update-tasks');
      }
    },
    completeTask() {
      this.$emit('complete-task', { task: this.task, index: this.index });
    }
  }
};
</script>
<style>
  .body {
    display: flex;
    justify-content: center;
    width: 100%;
    margin-top: 400px;
  }

  .board {
    display: flex;
    justify-content: space-between;
    width: 90%;
  }

  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: #000000;
    padding: 20px;
    border-radius: 9px;
    box-shadow: 0 2px 5px rgb(255, 255, 255);
    width: 400px;
    animation: glow 15s linear infinite;
  }

  .container__task{
    background-color: #000000;
    color: white;
    border: 0;
  }

  .container__button{
    background-color: #ffffff;
    color: rgb(0, 0, 0);
    text-shadow: none;
    outline: none;
    border: 0;
    border: 1px white;
    border-radius: 5px;
  }

  .container__text {
    color: white;

  }

  .input__group {
    display: flex;
    margin-bottom: 10px;
    border-radius: 10px;
    padding: 8px;
    border: 1px solid #ffffff;
    gap: 10px;
  }

  .completed__task {
    padding: 8px 16px;
    background-color: #000000;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  #task__list {
    list-style: none;
    padding: 0;
  }

  .button__delete{
    margin-left: 5px;
    padding: 8px 17px;
    background-color: #ffffff;
    border: 1px solid #ccc;
    border-radius: 4px;
    cursor: pointer;
  }

  .button__edit{
    margin-left: 5px;
    padding: 5px 10px;
    background-color: #ffffff;
    border: 1px solid #ccc;
    border-radius: 4px;
    cursor: pointer;
  }
  .task.completed {
    text-decoration: line-through;
    opacity: 0.5;
  }

  .actions {
    display: flex;
    margin-left: auto;
  }

  .actions button {
    margin-left: 5px;
    padding: 5px 10px;
    background-color: #ffffff;
    border: 1px solid #ccc;
    border-radius: 4px;
    cursor: pointer;
  }
  header {
    line-height: 1.5;
  }

  .logo {
    display: block;
    margin: 0 auto 2rem;
  }
    .logo {
      margin: 0 2rem 0 0;
    }

    header .wrapper {
      display: flex;
      place-items: flex-start;
      flex-wrap: wrap;
    }

.button__complete {
  margin-left: 5px;
  padding: 5px 10px;
  background-color: #ffffff;
  border: 1px solid #ccc;
  border-radius: 4px;
  cursor: pointer;
}

.task.completed {
  text-decoration: line-through;
  opacity: 0.5;
}

  @keyframes glow {
    10% { box-shadow: 0 0 10px rgb(255, 0, 0); }
      30% { box-shadow: 0 0 10px rgb(77, 0, 0); }
      60% { box-shadow: 0 0 20px rgba(255, 0, 0); }
      0% { box-shadow: 0 0 10px rgba(77, 0, 0); }
      90% { box-shadow: 0 0 20px rgba(255, 0, 0)}
      100% { box-shadow: 0 0 20px rgba(77, 0, 0); }

  }

  .ghost{
    opacity: 0.5;
    background-color: #c8ebfb;
  }

  .list-group-item{
    cursor: move;
  }

  .container2 {
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: #000000;
    padding: 20px;
    border-radius: 9px;
    box-shadow: 0 2px 5px rgb(255, 255, 255);
    width: 400px;
    animation: glow2 15s linear infinite;
  }

  @keyframes glow2 {
      10% { box-shadow: 0 0 10px rgba(81, 255, 0, 0.8); }
      30% { box-shadow: 0 0 10px rgba(9, 255, 0, 0.2); }
      60% { box-shadow: 0 0 20px rgba(81, 255, 0, 0.8); }
      0% { box-shadow: 0 0 10px rgba(9, 255, 0, 0.2); }
      90% { box-shadow: 0 0 20px rgba(81, 255, 0, 0.8); }
      100% { box-shadow: 0 0 20px rgba(9, 255, 0, 0.2); }

  }

  .container__task{
    background-color: #000000;
    color: white;
    border: 0;
  }

  .container__button{
    background-color: #ffffff;
    color: rgb(0, 0, 0);
    text-shadow: none;
    outline: none;
    border: 0;
    border: 1px white;
    border-radius: 5px;
  }

  .container__text {
    color: white;

  }
   .clear-button {
     background: none;
     border: none;
     cursor: pointer;
     font-size: 16px;
     color: #ffffff;
     margin-left: -30px;
     margin-top: 5px;
   }

   .button__delete{
     max-width: 90px;
     font-size: 9px;
     margin-top: auto;
   }

  .column {
    background-color: #000000;
    padding: 20px;
    border-radius: 9px;
    box-shadow: 0 2px 5px rgb(255, 255, 255);
    width: 22%;
  }

  .column__title {
    color: white;
    text-align: center;
    margin-bottom: 20px;
  }

  .body {
    display: flex;
    justify-content: center;
    width: 100%;
    margin-top: 50px;
  }

  .board {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
    width: 90%;
  }

  .column {
    background-color: #000000;
    padding: 20px;
    border-radius: 9px;
    box-shadow: 0 2px 5px rgb(255, 255, 255);
    width: 22%;
  }

  .column__title {
    color: white;
    text-align: center;
    margin-bottom: 20px;
  }

  .add-list-form {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 20px;
  }

  .add-list-form input {
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    margin-right: 10px;
  }

  .add-list-form button {
    padding: 10px 20px;
    background-color: #000000;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  .add-list-form button:hover {
    background-color: #555555;
  }

  .add-task-form {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 10px;
  }

  .add-task-form input {
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    margin-right: 10px;
  }

  .add-task-form button {
    padding: 10px 20px;
    background-color: #000000;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  .add-task-form button:hover {
    background-color: #555555;
  }

  .column__header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .column__actions {
    display: flex;
    gap: 5px;
  }
  .column__actions button {
    padding: 5px 10px;
    background-color: #ffffff;
    border: 1px solid #ccc;
    border-radius: 4px;
    cursor: pointer;
  }
</style>



