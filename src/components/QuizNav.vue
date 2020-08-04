<template>
  <div class="quiz-nav">
    <button v-show="index > 0" @click="handleBack" :disabled="!allowBack">Back</button>
    <button v-show="index + 1 < count" @click="handleNext" :disabled="!allowNext">Next</button>
    <button v-show="count == index + 1" @click="handleSubmit" :disabled="!allowBack || !allowNext">Submit</button>
  </div>
</template>

<script>
export default {
  name: "QuizNav",
  props: {
    index: Number,
    count: Number,
    allowNext: Boolean,
    allowBack: {
      type: Boolean,
      default: true,
    },
  },
  methods: {
    handleBack() {
      if (this.allowBack) this.$emit("onBack");
    },
    handleNext() {
      if (this.allowNext) this.$emit("onNext");
    },
    handleSubmit() {
      this.$emit("onSubmit");
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
