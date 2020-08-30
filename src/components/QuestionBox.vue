<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{ currentQuestion.question}}</template>

      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item 
        v-for="(answer, index) in showAnswers" 
        :key="index"
        >
          {{answer}}
        </b-list-group-item>
      </b-list-group>

      <b-button variant="primary" href="#">Submit</b-button>
      <b-button v-on:click="onClicNext" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template> 

<script>
export default {
  props: {
    currentQuestion: Object,
    onClicNext: Function
  },
  computed: {
    showAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  }
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover {
  background: #eee;
  cursor: pointer;
}
.btn {
  margin: 0px 5px;
}
</style>