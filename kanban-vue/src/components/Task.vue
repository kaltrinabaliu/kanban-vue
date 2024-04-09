<template>
  <div class="single-task" :class="className" :draggable="true" @dragstart="onDragStartHandler"
    @dragenter="onDragEnterHandler">
    <h5 class="task-title">{{ task.title }}</h5>
    <p class="task-desc">{{ task.desc }}</p>
  </div>
</template>

<script>
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
  computed: {
    className() {
      return this.dragging ? this.getStyles({ tableIndex: this.tableIndex, taskIndex: this.taskIndex }) : 'single-task';
    }
  },
  methods: {
    onDragStartHandler(e) {
      this.handleDragStart(e, { tableIndex: this.tableIndex, taskIndex: this.taskIndex });
    },
    onDragEnterHandler() {
      if (this.dragging) {
        this.handleDragEnter(null, { tableIndex: this.tableIndex, taskIndex: this.taskIndex });
      }
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
    opacity: 1; 
  }
  
  .current .task-desc{
    color: #7e7e84; 
  }
  
  .current .task-title {
    color: #7e7e84; 
  }
  </style>