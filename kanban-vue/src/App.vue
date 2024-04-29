<template>
  <div>
    <Kanban :data="data" />
  </div>
</template>

<script>
import dummyData from './assets/data.json';
import Kanban from './components/Kanban.vue';

export default {
  data() {
    return {
      data: dummyData
    };
  },
  components: {
    Kanban
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.container {
  width: 80%;
  margin: 0 auto;
}
</style>
