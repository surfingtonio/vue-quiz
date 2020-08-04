<template>
  <div id="app">
    <p class="results" v-show="complete">
      <strong v-if="score >= count/2">Congratulations!</strong>
      <strong v-else>Sorry!</strong>
      You got {{score}} out of {{count}} questions.
    </p>
    <quiz-item
      v-show="index == i"
      v-for="(item, i) in items"
      :key="i"
      :item="item"
      :status="status"
      :chosen="answers[index]"
      @onAnswerSelected="handleAnswerSelected"
    />
    <quiz-nav
      :count="count"
      :index="index"
      :allowNext="allowNext"
      :allowBack="!complete"
      @onNext="handleNext"
      @onBack="handleBack"
      @onSubmit="handleSubmit"
    />
  </div>
</template>

<script>
import axios from "axios";
import QuizItem from "./components/QuizItem.vue";
import QuizNav from "./components/QuizNav.vue";

export default {
  name: "App",
  components: {
    QuizItem,
    QuizNav,
  },
  data() {
    return {
      items: [],
      answers: [],
      count: 0,
      index: 0,
      score: 0,
      complete: false,
      allowNext: false,
    };
  },
  created() {
    axios
      .get("https://opentdb.com/api.php?amount=5&category=9&difficulty=easy")
      .then((res) => {
        this.items = res.data.results;
        this.count = this.items.length;
      })
      .catch((error) => console.error(error));
  },
  methods: {
    handleBack() {
      this.allowNext = true;
      this.index--;
    },
    handleNext() {
      this.index++;
      this.allowNext = this.answers[this.index] !== undefined;
    },
    handleSubmit() {
      this.score = this.answers.reduce(
        (a, c, i) => (a += this.items[i].correct_answer == c ? 1 : 0),
        0
      );
      this.complete = true;
    },
    handleAnswerSelected(e) {
      this.$set(this.answers, this.index, e);
      this.allowNext = true;
    },
  },
  computed: {
    status: function () {
      return `Question ${this.index + 1} of ${this.count}`;
    },
  },
};
</script>

<style>
:root {
  --text-color: #202124;
  --button-color: #fff;
  --button-background: #1a73e8;
  --button-disabled-background: #1a73e8aa;
  --button-hover-background: #2d53af;
  --border-radius: 0.25rem;
  --choice-background: #fff;
  --choice-border-color: #eee;
  --choice-color: #3c4043;
  --choice-hover-background: #f5f5f5;
  --choice-hover-color: #1a73e8;
  --chosen-background: #4285f4;
  --question-color: #202124;
}
#app {
  font-family: Lato, sans-serif;
  margin: 1rem auto;
  min-width: 40rem;
  padding: 1rem;
  width: 50%;
}
.results {
  border: 2px solid var(--chosen-background);
  border-radius: var(--border-radius);
  font-size: 1.5rem;
  padding: 1rem;
  text-align: center;
}
</style>
