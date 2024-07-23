<template>
  <div>
    <draggable
      v-model="filteredItems"
      :group="{ name: groupName, pull: true, put: true }"
      :itemKey="itemKey"
      @end="updateTasks"
    >
      <template #item="{ element, index }">
        <TodoItem
          :task="element"
          :index="index"
          @delete-task="onDeleteTask(index)"
          @update-tasks="saveTasks"
          @complete-task="completeTask({ task: element, listId: listId, index: index })"
        />
      </template>
    </draggable>
    <div class="add-task-form">
      <input type="text" v-model="newTaskText" placeholder="New task" />
      <button @click="addTask">Add Task</button>
    </div>
  </div>
</template>

<script>
import TodoItem from "@/components/TodoItem.vue";
import draggable from "vuedraggable";

export default {
  
  components: {
    TodoItem,
    draggable
  },

  name: "List",

  props: {
    items: {
      type: Array,
      required: true
    },
    saveTasks: {
      type: Function,
      required: true
    },
    deleteTask: {
      type: Function,
      required: true
    },
    completeTask: {
      type: Function,
      required: true
    },
    groupName: {
      type: String,
      required: true
    },
    listId: {
      type: Number,
      required: true
    },
    itemKey: {
      type: String,
      required: true
    }
  },

  data() {
    return {
      localItems: [...this.items],
      newTaskText: '',
      searchText: '',
      filtered:' ',
    }
  },

  computed: {
    filteredItems() {
      return this.items.filter(item => this.matchesSearch(item));
    }
  },
  watch: {
    items: {
      handler(newItems) {
        this.localItems = [...newItems];

      },
      deep: true,
      immediate: true
    }
  },

  methods: {
    updateTasks() {
      this.$emit('update:items', this.localItems);
      this.saveTasks();
    },
    
    addTask() {
      if (this.newTaskText.trim()) {
        this.localItems.push({ text: this.newTaskText.trim(), completed: false });
        const newTask = { text: this.newTaskText.trim(), completed: false };
        this.localItems.push(newTask); 
        this.updateTasks();
        
        this.newTaskText = '';
        this.saveTasks();
      }
    },

    onDeleteTask(index) {
      this.localItems.splice(index, 1);
      this.updateTasks();
    },
  }
}
</script>

<style>
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
</style>
