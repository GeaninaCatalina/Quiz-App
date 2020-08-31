<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{ currentQuestion.question}}</template>

      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item 
        v-for="(answer, index) in showAnswers" 
        :key="index"
        @click="selectAnswer(index)"
        :class="[selectedIndex === index ? 'selected-answer': '']"
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
  data(){
    return {
      selectedIndex: null
    }
  },
  computed: {
    showAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  }, 
  watch: {
    currentQuestion(){
      this.selectAnswer = null;
      this.suffleAnswers();
    }
  },
  methods:{
    selectAnswer(index) {
      this.selectedIndex = index; 
      console.log(index);
    },
    suffleAnswers(){
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
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

.selected-answer{
background-color:lightblue;
}

.correct-answer{
background-color: darkseagreen;
}

.incorrect-answer{
background-color: firebrick;
}
</style>