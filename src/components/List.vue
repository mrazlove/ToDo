<template>
  <div>
    <draggable
        v-model="localItems"
        :group="{ name: groupName, pull: true, put: true }"
        :itemKey="itemKey"
        @end="updateTasks"
    >
      <template #item="{element, index}">
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
      newTaskText: ''
    };
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
        this.newTaskText = '';
        this.updateTasks();
      }
    },

    onDeleteTask(index) {
      this.deleteTask(this.listId, index);
    }
  }
}
</script>

<style>
</style>
