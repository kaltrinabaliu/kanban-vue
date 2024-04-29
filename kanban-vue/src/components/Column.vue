<template>
  <div 
    class="single-table" 
    :style="{ border: isDraggingOver ? '5px solid rgb(68, 115, 243)' : '2px solid #616060' }"
    @dragover="handleDragOver"
    @dragleave="handleDragLeave"
    @drop="handleDrop"
    @dragenter="dragging && !tasks.length ? handleDragEnter($event, { tableIndex, taskIndex: 0 }) : undefined"
  >
    <div class="table-title" :style="{ backgroundColor: tableTitleColor() }">
      <h2 :style="{ margin: '24px' }">{{ status }}</h2>
<h2 class="tasks-length">[{{ tasks.length }}]</h2>
    </div>
    <div class="table-content">
      <TaskComponent
        v-for="(task, taskIndex) in tasks.slice(0, getDisplayedTasksCount())"
        :key="task.id"
        :task="task"
        :tableIndex="tableIndex"
        :taskIndex="taskIndex"
        :dragging="dragging"
        :handleDragStart="handleDragStart"
        :handleDragEnter="handleDragEnter"
        :getStyles="getStyles"
      />
      <button v-if="getDisplayedTasksCount() < tasks.length" @click="handleLoadMore" type="button" class="load-more">Load More</button>
    </div>
  </div>
</template>

<script>
import TaskComponent from './Task.vue';
import {ref} from 'vue';
export default {
  name: "the-column",
  props: {
    status: String,
    tasks: Array,
    tableIndex: Number,
    dragging: Boolean,
    handleDragEnter: Function,
    handleDragStart: Function,
    getStyles: Function,
  },
  components: {
    TaskComponent,
  },
  setup(props) {
    const displayedTasks = ref({});
    const isDraggingOver = ref(false);

    function handleLoadMore() {
      displayedTasks.value[props.tableIndex] = (displayedTasks.value[props.tableIndex] || 4) + 2;
    }

    function handleDragOver(event) {
  event.preventDefault();
  isDraggingOver.value = true;
  const target = event.target;

  // Check if target exists
  if (
    target &&
    !target.classList.contains("table-content") &&
    isDraggingOver.value &&
    props.dragging
  ) {
    const rect = target.getBoundingClientRect();
    const distanceFromTop = event.clientY - rect.top;
    const bottomMargin = rect.height * 0.6;

    if (distanceFromTop >= rect.height - bottomMargin) {
      const lastTask = target.querySelector('.table-content .single-task:last-child');
      if (lastTask) {
        const taskRect = lastTask.getBoundingClientRect();
        const taskBottom = taskRect.bottom;
        const nearBottom = event.clientY >= taskBottom;
        if (nearBottom) {
          // Insert after the last task
          props.handleDragEnter(event, { tableIndex: props.tableIndex, taskIndex: props.tasks.length - 1 });
          return;
        }
      }
    }
  }
}


    function handleDragLeave(event) {
      const relatedTarget = event.relatedTarget;
      const tableElement = event.currentTarget;
      if (!tableElement.contains(relatedTarget)) {
        isDraggingOver.value = false;
      }
    }

    function handleDrop() {
      isDraggingOver.value = false;
    }

    function getDisplayedTasksCount() {
      return displayedTasks.value[props.tableIndex] || 4;
    }

    function tableTitleColor() {
      let color;
      switch (props.status) {
        case 'TO DO':
          color = 'red';
          break;
        case 'IN PROGRESS':
          color = '#9f950a';
          break;
        case 'DONE':
          color = 'green';
          break;
        case 'FINAL':
          color = 'blue';
          break;
        default:
          color = 'pink';
      }
      return color;
    }

    return {
      isDraggingOver,
      handleLoadMore,
      handleDragOver,
      handleDragLeave,
      handleDrop,
      getDisplayedTasksCount,
      tableTitleColor
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
  display: flex;
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

.tasks-length{
    margin-top: 20px;
      }
</style>
