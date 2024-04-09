<template>
  <div class="single-table" @dragenter="handleDragEnter">
    <div class="table-title" :style="{ backgroundColor: tableTitleColor }">
      <h2>{{ status }}</h2>
    </div>
    <div class="table-content" @dragover="setHighlighted(true)" @dragleave="setHighlighted(false)">
      <TaskComponent v-for="(task, taskIndex) in tasks.slice(0, getDisplayedTasksCount())" :key="task.id"
        :task="task" :tableIndex="tableIndex" :taskIndex="taskIndex" :dragging="dragging"
        @handleDragStart="handleDragStart" @handleDragEnter="handleDragEnter" :getStyles="getStyles" />
      <button v-if="getDisplayedTasksCount() < tasks.length" @click="handleLoadMore" class="load-more">Load More</button>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
import TaskComponent from './Task.vue';

export default {
  name:"the-column",
  props: {
    status: String,
    tasks: Array,
    tableIndex: Number,
    dragging: Boolean,
    handleDragEnter: Function,
    handleDragStart: Function,
    getStyles: Function
  },
  components: {
    TaskComponent
  },
  setup(props) {
    const displayedTasks = ref({});
    const highlighted = ref(false);

    const handleLoadMore = () => {
      displayedTasks.value[props.tableIndex] = (displayedTasks.value[props.tableIndex] || 4) + 2;
    };

    const getDisplayedTasksCount = () => {
      return displayedTasks.value[props.tableIndex] || 4;
    };

    const tableTitleColor = (() => {
      switch (props.status) {
        case 'TO DO':
          return 'red';
        case 'IN PROGRESS':
          return 'green';
        case 'DONE':
          return 'yellow';
        case 'FINAL':
          return 'blue';
        default:
          return 'pink';
      }
    })();

    const setHighlighted = (value) => {
      highlighted.value = value;
    };

    return {
      displayedTasks,
      highlighted,
      handleLoadMore,
      getDisplayedTasksCount,
      tableTitleColor,
      setHighlighted
    };
  }
};
</script>
  
  <style scoped>
  .single-table {
    display: flex;
    flex-direction: column;
    width: 100%;
    overflow: hidden;
    border-radius: 10px;
    border: 2px solid #616060;
    height: 800px;
    background-color: #dddedf;
  }
  
  .table-title {
    padding: 1rem;
    text-align: center;
    color: #f2f5f7;
  }
  
  .table-content {
    padding: 1rem;
    background-color: #dddedf;
    overflow-y: scroll;
    scrollbar-width: none;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  
  .table-content .load-more {
    border: none;
    padding: 0.75rem 1.5rem;
    border-radius: 8px;
    cursor: pointer;
  }
  
  .highlighted {
    border: 2px solid rgb(137, 137, 138); 
    border-radius: 5px;
    background-color: rgb(199, 199, 199);
    height: 800px;
  }
  </style>