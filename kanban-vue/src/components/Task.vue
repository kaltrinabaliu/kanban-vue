<template>
  <div :class="className" :draggable="true" @dragstart="onDragStartHandler"
    @dragenter="onDragEnterHandler">
    <h5 class="task-title">{{ task.title }}</h5>
    <p class="task-desc">{{ task.desc }}</p>
  </div>
</template>

<script>
import {  computed } from 'vue';
export default {
  name:"the-task",
  props: {
    task: Object,
    tableIndex: Number,
    taskIndex: Number,
    dragging: Boolean,
    handleDragStart: Function,
    handleDragEnter: Function,
    getStyles: Function
  },

  setup(props){

    const className = computed(() => {
      return props.dragging ? props.getStyles({ tableIndex: props.tableIndex, taskIndex: props.taskIndex }) : 'single-task';
      });
   function onDragStartHandler(e) {
    props.handleDragStart(e, { tableIndex: props.tableIndex, taskIndex: props.taskIndex });
    }
  function  onDragEnterHandler(event) {
      if (props.dragging) {
        props.handleDragEnter(event, { tableIndex: props.tableIndex, taskIndex: props.taskIndex });
      }
      undefined;
    }

    return{
className,
onDragStartHandler,
onDragEnterHandler

    }
  }
};
</script>
  
  <style scoped>
  .single-task {
    width: 100%;
    padding: 2rem;
    margin-bottom: 1rem;
    border-radius: 8px;
    cursor: pointer;
    background-color: #f0ede4;
    transition: all 0.3s ease-in-out;
  }
  
  .single-task:last-child {
    margin-bottom: 0;
  }
  
  .task-title {
    font-size: 1.3rem;
    color: #282c34;
  }
  
  .task-desc {
    margin-top: 1rem;
    font-size: 1rem;
    color: #282c34
  }
  
  .current {
    background-color: #7e7e84; 
    opacity: 0.5; 
    width: 100%;
    padding: 2rem;
    margin-bottom: 1rem;
    border-radius: 8px;
    cursor: pointer;
  }
  
  .current .task-desc{
    color: #7e7e84; 
  }
  
  .current .task-title {
    color: #7e7e84; 
  }
  </style>