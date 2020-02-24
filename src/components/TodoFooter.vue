<template>
  <div class="clearAllContainer">
    <span class="clearAllBtn" @click="clearTodoAll">Clear All</span>
  </div>
</template>

<script>
export default {
  methods: {
    clearTodoAll() {
      //로컬스토리지의 목록 지우기
      //localStorage.clear();
      this.$emit('removeAll');
    }
  }
}
</script>

<style scoped>
  .clearAllContainer {
    width: 8.5rem;
    height: 50px;
    line-height: 50px;
    background-color: white;
    border-radius: 5px;
    margin: 0 auto;
  }
  .clearAllBtn {
    cursor: pointer;
    color: #e20303;
    display: block;
  }
</style>
