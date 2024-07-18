<template>
  <div class="body">
    <div class="board">
      <div class="add-list-form">
        <input type="text" v-model="newListName" placeholder="New list name" />
        <button @click="addList">Add List</button>
      </div>
      <div v-for="list in lists" :key="list.id" class="column">
        <div class="column__header">
          <h2 class="column__title">{{ list.name }}</h2>
          <div class="column__actions">
            <button @click="editList(list.id)">Edit</button>
            <button @click="deleteList(list.id)">Delete</button>
          </div>
        </div>
        <List
            :items="list.tasks"
            :save-tasks="saveTasks"
            :delete-task="deleteTask"
            :complete-task="completeTask"
            :group-name="list.name"
            :list-id="list.id"
            :item-key="'text'"
            @update:items="updateTasks(list.id)"
        />
      </div>
    </div>
  </div>
</template>

<script>
import List from "@/components/List.vue";

export default {
  components: {
    List
  },

  data() {
    return {
      newListName: "",
      lists: [
        { id: 1, name: 'To Do', tasks: [] },
        { id: 2, name: 'In Progress', tasks: [] },
        { id: 3, name: 'Review', tasks: [] },
        { id: 4, name: 'Done', tasks: [] },
      ]
    };
  },

  methods: {
    getTasksByStatus(status) {
      const list = this.lists.find(list => list.name === status);
      return list ? list.tasks : [];
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
</style>