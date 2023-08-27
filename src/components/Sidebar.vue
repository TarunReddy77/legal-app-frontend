<template>
  <div class="sidebar border-dark-subtle border-end w-25 d-none d-md-flex flex-column">
    <h3 class="lead display-6 text-center text-primary fw-semibold mb-3">History</h3>
    <div class="d-flex align-items-center my-3" v-for="(question, index) in userQuestions" :key="index">
      <button class="btn btn-outline-danger me-3" @click="deleteQuestion(index)">
        <i class="bi bi-trash3"></i>
      </button>
      <div class="lead">{{ question }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Sidebar',
  props: {
    userQuestions: Array // List of user questions passed as a prop
  },
  methods: {
    deleteQuestion(index) {
      this.$emit('deleteQuestion', index);
    }
  }
};
</script>

<style scoped>
.sidebar {
  //width: 400px;
  //background-color: #f8f9fa;
  padding: 20px;
  //border-right: 1px solid #dee2e6;
}
</style>
