<template>
  <div class="board-container">
    <div class="container">
      <div class="board-content">
        <Column v-for="(table, tableIndex) in dataLists" 
          :key="table.id"
          :status="table.status" 
          :tasks="table.tasks" 
          :tableIndex="tableIndex"
          :dragging="dragging" 
          :handleDragEnter="handleDragEnter"
          :handleDragStart="handleDragStart" 
          :getStyles="getStyles" />
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
import Column from './Column.vue';
import { StatusEnum } from '../types/index'; 

export default {
  name:"the-kanban",
  props: {
    data: Array
  },
  components: {
    Column
  },
  setup(props) {
    const dataLists = ref(props.data);
    const dragging = ref(false);

    const dragItem = ref(null);
    const dragNode = ref(null);

    function handleDragStart (event, task)  {
      dragItem.value = task;
      dragNode.value = event.target;
      if (dragNode.value) {
        dragNode.value.addEventListener('dragend', handleDragEnd);
      }
      setTimeout(() => {
        dragging.value = true;
      }, 0);
    }

    function handleDragEnter (event, task) {
  const currentItem = dragItem.value;

  const draggedTask = dataLists.value[currentItem.tableIndex].tasks[currentItem.taskIndex];

  if (event.target !== dragNode.value) {
    if (dataLists.value[task.tableIndex].status === StatusEnum.FINAL && draggedTask.id % 2 === 0) {
      return;
    }

    const newList = JSON.parse(JSON.stringify(dataLists.value));
    const removedTask = newList[currentItem.tableIndex].tasks.splice(currentItem.taskIndex, 1)[0];
    newList[task.tableIndex].tasks.splice(task.taskIndex, 0, removedTask);
    dragItem.value = { ...currentItem, tableIndex: task.tableIndex, taskIndex: task.taskIndex };
    dataLists.value = newList;
  }
}

    function handleDragEnd () {
      dragging.value = false;
      if (dragNode.value) {
        dragNode.value.removeEventListener('dragend', handleDragEnd);
      }
      dragItem.value = null;
      dragNode.value = null;
      
    }

    const getStyles = (task) => {
      const currentItem = dragItem.value;
      if (currentItem && currentItem.tableIndex === task.tableIndex && currentItem.taskIndex === task.taskIndex) {
        return 'current single-task';
    }
    return 'single-task';
  };

    return {
      dataLists,
      dragging,
      handleDragEnter,
      handleDragStart,
      getStyles
    };
  }
};
</script>
  
  <style scoped>
  .board-container {
    background-color: #1f1f1f;
  }
  
  .board-content {
    padding: 3rem 0;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    gap: 2rem;
    min-height: 100vh;
  }
  </style>
  