<template>
  <div id="app">
    <Header :numCorrect="numCorrect" :numTotal="numTotal" />
    <b-container class="bv-example-row">
      <b-row>
        <b-col offset="3" sm="6">
          <QuestionBox
            :questionNum="questionNum"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
        /></b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0,
      questionNum: 1,
      numCorrect: 0,
      numTotal: 0,
    };
  },
  methods: {
    next() {
      this.index++;
      this.questionNum++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.numTotal++;
    },
  },
  mounted() {
    const uri =
      "https://opentdb.com/api.php?amount=15&category=21&difficulty=easy&type=multiple";
    fetch(uri, {
      method: "get",
    })
      .then((response) => {
        return response.json();
      })
      .then((data) => {
        this.questions = data.results;
      });
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
