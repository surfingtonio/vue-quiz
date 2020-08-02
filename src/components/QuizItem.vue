<template>
  <div class="quiz-item">
    <div>
      <p class="status">{{status}}</p>
      <p class="question" v-html="item.question" />
    </div>
    <ul>
      <li
        class="choice"
        v-for="(choice, i) in choices"
        :key="i"
        :class="{chosen: choice === chosen}"
      >
        <label>
          <input type="radio" :name="name" :value="choice" @change="handleChange" />
          <span v-html="choice" />
        </label>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "QuizItem",
  props: {
    item: Object,
    chosen: String,
    status: String,
  },
  data() {
    return {
      choices: [this.item.correct_answer, ...this.item.incorrect_answers].sort(
        () => Math.random() - 0.5
      ),
      name: `item.${Math.floor(Math.random() * 1000000)}`,
    };
  },
  methods: {
    handleChange: function (e) {
      this.$emit("onAnswerSelected", e.target.value);
    },
  },
};
</script>

<style scoped>
.quiz-item {
  margin: 1rem;
}
.quiz-item div {
  margin: 1rem 0 1.5rem;
  padding: 1rem 0;
}
.quiz-item .status {
  text-align: right;
  margin-bottom: 1.5rem;
}
.quiz-item .question {
  color: var(--question-color);
  font-size: 2rem;
  text-align: center;
}
.quiz-item ul {
  padding: 0;
}
.quiz-item .choice {
  background: var(--choice-background);
  border: 2px solid var(--choice-border-color);
  border-radius: var(--border-radius);
  color: var(--choice-color);
  cursor: pointer;
  font-size: 1.125rem;
  list-style: none;
  margin: 0.5rem 0;
}
.quiz-item .choice:hover {
  color: var(--choice-hover-color);
  border: 2px solid var(--choice-hover-color);
}
.quiz-item .choice.chosen {
  background: var(--chosen-background);
  border: 2px solid var(--chosen-background);
  color: var(--choice-background);
}
.quiz-item li label {
  cursor: pointer;
  display: block;
  padding: 1rem;
}
.quiz-item li input {
  cursor: pointer;
  margin-right: 1rem;
}
</style>
