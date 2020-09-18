<template>
  <div class="wrapper">
    <b-jumbotron v-if="currentQuestion">
      <h5>Question {{ questionNum }}</h5>
      <template>
        {{ currentQuestion.question }}
      </template>

      <hr />
      <!-- Start Answers -->
      <b-button
        variant="none"
        block
        v-for="(answer, index) in shuffledAnswers"
        :key="index"
        @click.prevent="selectAnswer(index)"
        :class="answerClass(index)"
      >
        {{ answer }}</b-button
      >
      <!-- End Answers -->
      <b-button-group class="mt-2">
        <b-button
          variant="warning"
          href="#"
          @click="submitAnswer"
          :disabled="selectedIndex === null || submitted"
          >Submit</b-button
        >
        <b-button variant="info" href="#" @click="next">Next</b-button>
      </b-button-group>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  name: "QuestionBox",
  props: {
    questionNum: Number,
    currentQuestion: Object,
    next: Function,
    increment: Function,
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
      correctIndex: null,
      submitted: false,
    };
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.submitted = false;
        this.shuffleAnswers();
      },
    },
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }

      this.submitted = true;
      this.increment(isCorrect);
    },
    answerClass(index) {
      let answerClass = "";
      if (this.selectedIndex == null) {
        answerClass = "btn-outline-info";
      } else if (!this.submitted && this.selectedIndex === index) {
        answerClass = "btn-info";
      } else if (this.submitted && this.correctIndex === index) {
        answerClass = "btn-success";
      } else if (
        this.submitted &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      ) {
        answerClass = "btn-danger";
      }

      return answerClass;
    },
  },
};
</script>

<style scoped>
.wrapper {
  margin-top: 40px;
}
</style>
