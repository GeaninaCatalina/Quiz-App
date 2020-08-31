<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{ currentQuestion.question}}</template>

      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in suffleAnswers"
          :key="index"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
        >{{answer}}</b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        @click="onSubmit"
        :disabled="selectedIndex === null || answered"
      >Submit</b-button>
      <b-button v-on:click="onClicNext" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template> 

<script>
import _ from "lodash";

export default {
  props: {
    currentQuestion: Object,
    onClicNext: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      suffleAnswers: [],
      answered: false
    };
  },
  computed: {
    showAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.suffleAnswersList();
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
      console.log(index);
    },

    onSubmit() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    },
    suffleAnswersList() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      this.suffleAnswers = _.shuffle(answers);
      this.correctIndex = this.suffleAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    answerClass(index) {
      let answerClass = ''; 

       if (!this.answered && this.selectedIndex === index) {
         answerClass = "selected-answer"
       } else if (this.answered && this.correctIndex === index) {
         answerClass = "correct-answer"
       } else if (this.answered && this.selectedIndex === index && this.selectedIndex === index) {
         answerClass ="incorrect-answer"
       } else {
         answerClass = ""; 
       }
      return answerClass; 
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

.selected-answer {
  background-color: lightblue;
}

.correct-answer {
  background-color: darkseagreen;
}

.incorrect-answer {
  background-color: firebrick;
}
</style>