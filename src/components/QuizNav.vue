<template>
  <div class="quiz-nav">
    <button v-show="backShown" @click="handleBack" :disabled="backDisabled">Back</button>
    <button v-show="nextShown" @click="handleNext" :disabled="nextDisabled">Next</button>
    <button v-show="submitShown" @click="handleSubmit" :disabled="submitDisabled">Submit</button>
  </div>
</template>

<script>
export default {
  name: "QuizNav",
  props: {
    index: Number,
    count: Number,
    answered: Boolean,
    complete: Boolean,
  },
  methods: {
    handleBack() {
      if (!this.backDisabled) this.$emit("onBack");
    },
    handleNext() {
      if (!this.nextDisabled) this.$emit("onNext");
    },
    handleSubmit() {
      this.$emit("onSubmit");
    },
  },
  computed: {
    backDisabled: function () {
      return this.complete;
    },
    backShown: function () {
      return this.index > 0;
    },
    nextDisabled: function () {
      return !this.answered;
    },
    nextShown: function () {
      return this.index + 1 < this.count;
    },
    submitDisabled: function () {
      return !this.answered || this.complete;
    },
    submitShown: function () {
      return this.count == this.index + 1;
    },
  },
};
</script>

<style scoped>
.quiz-nav {
  align-items: center;
  display: flex;
  justify-content: center;
}
.quiz-nav button {
  background: var(--button-background);
  border: none;
  border-radius: var(--border-radius);
  color: var(--button-color);
  cursor: pointer;
  margin: 0 var(--border-radius);
  padding: 0.75rem 1.5rem;
  outline: none;
}
.quiz-nav button:hover {
  background: var(--button-hover-background);
}
.quiz-nav button:disabled {
  background: var(--button-disabled-background);
}
</style>
