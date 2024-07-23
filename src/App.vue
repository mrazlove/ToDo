<template>
  <div class="body">
    <div class="board">
      <div class="add-list-form">
        <input type="text" v-model="searchText" placeholder="Search tasks" @input="filterTasks" />
        <input type="text" v-model="newListName" placeholder="New list name" />
        <button @click="addList">Add List</button>
      </div>
      <div v-for="list in filteredLists" :key="list.id" class="column">
        <div class="column__header">
          <h2 class="column__title">{{ list.name }}</h2>
          <div class="column__actions">
            <button @click="editList(list.id)">Edit</button>
            <button @click="deleteList(list.id)">Delete</button>
          </div>
        </div>
        <input type="text" v-model="searchText" placeholder="Search tasks" />
          <List
          v-for="(list, index) in filteredLists"
          :key="list.id"
          :items="list.tasks"
          :save-tasks="saveTasks"
          :delete-task="deleteTask"
          :complete-task="completeTask"
          :group-name="groupName"
          :list-id="list.id"
          :item-key="'text'"
          :search-text="searchText"
          @update-items="updateItems"
          />
      </div>
  </div>
  </div>
</template>
<script>
import List from "@/components/List.vue";

export default {
  watch: {
    searchText() {
      this.filteredTasks = this.tasks.filter(task => task.text.toLowerCase().includes(this.searchText.toLowerCase()));
    }
  },
  components: {
    List
  },

  data() {
    return {
      newListName: "",
      searchText: "",
      lists: [
        { id: 1, name: 'To Do', tasks: [] },
        { id: 2, name: 'In Progress', tasks: [] },
        { id: 3, name: 'Review', tasks: [] },
        { id: 4, name: 'Done', tasks: [] },
      ]
    };
  },

  computed: {
    filteredLists() {
      const searchLower = this.searchText.toLowerCase();
      return this.lists.map(list => ({
        ...list,
        tasks: list.tasks.filter(task => task.text.toLowerCase().includes(searchLower))
      }));
    },
  },

  methods: {
    filterTasks() {

    },
    
    updateTasks(listId) {
      return (newTaskList) => {
        const list = this.lists.find(list => list.id === listId);
        if (list) {
          list.tasks = newTaskList;
          this.saveTasks();
        }
      };
    },

    deleteTask(listId, index) {
      const list = this.lists.find(list => list.id === listId);
      if (list) {
        list.tasks.splice(index, 1);
        this.saveTasks();
      }
    },

    completeTask({ task, listId, index }) {
      const list = this.lists.find(list => list.id === listId);
      if (list) {
        task.completed = !task.completed;
        list.tasks.splice(index, 1);
        if (task.completed) {
          list.tasks.push(task);
        } else {
          list.tasks.unshift(task);
        }
        this.saveTasks();
      }
    },

    addList() {
      if (this.newListName.trim()) {
        const newList = {
          id: Date.now(),
          name: this.newListName.trim(),
          tasks: []
        };
        this.lists.push(newList);
        this.newListName = "";
        this.saveTasks();
      }
    },

    saveTasks() {
      localStorage.setItem('todoLists', JSON.stringify(this.lists));
    },

    loadTasks() {
      const savedLists = localStorage.getItem('todoLists');
      if (savedLists) {
        this.lists = JSON.parse(savedLists);
      }
    },

    editList(listId) {
      const list = this.lists.find(list => list.id === listId);
      if (list) {
        const newName = prompt("Edit list name:", list.name);
        if (newName !== null && newName.trim() !== "") {
          list.name = newName.trim();
          this.saveTasks();
        }
      }
    },

    deleteList(listId) {
      this.lists = this.lists.filter(list => list.id !== listId);
      this.saveTasks();
    }
  },

  created() {
    this.loadTasks();
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
    flex-wrap: wrap;
    gap: 20px;
  }

  .add-list-form {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 20px;
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
  </style>