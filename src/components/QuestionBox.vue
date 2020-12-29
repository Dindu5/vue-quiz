<template>
  <div class="container">
    <div class="tile is-ancestor">
      <div class="tile is-vertical">
        <div class="tile">
          <div class="tile is-parent is-vertical">
            <article class="tile is-child box">
              <!-- <p class="title has-text-dark">Question 1</p> -->
              <p class="subtitle has-text-dark is-5">
                {{ currentQuestion.question }}
              </p>
              <hr />
              <article
                v-for="(option, index) in shuffledAnswers"
                :key="index"
                @click="selectedAnswer(option)"
                class="tile is-child notification p-2 has-background-grey answer"
                :class="answerClass(option)"
              >
                <p class="subtitle is-6" v-text="option"></p>
              </article>
              <div class="buttons">
                <button
                  class="button is-dark is-outlined"
                  @click="submitAnswer"
                  :disabled="selected === null || answered"
                >
                  Submit
                </button>
                <button class="button is-dark" @click="next">Next</button>
              </div>
            </article>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "QuestionBox",
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
  },
  data() {
    return {
      selected: null,
      shuffledAnswers: [],
      answered: false,
    };
  },
  methods: {
    // Getting and selecting answers
    selectedAnswer(option) {
      this.selected = option;
    },

    // Shuffling answer array
    shuffleAnswers(array) {
      const newArr = array.slice();
      for (let i = newArr.length - 1; i > 0; i--) {
        const rand = Math.floor(Math.random() * (i + 1));
        [newArr[i], newArr[rand]] = [newArr[rand], newArr[i]];
      }
      this.shuffledAnswers = newArr;
    },

    // Submitting Answers
    submitAnswer() {
      let isCorrect = false;
      if (this.selected === this.currentQuestion.correct_answer) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    },

    // Getting AnswerClass
    answerClass(option) {
      let answerClass = "";
      if (!this.answered && this.selected === option) {
        answerClass = "selected";
      } else if (
        this.answered &&
        this.currentQuestion.correct_answer === option
      ) {
        answerClass = "correct";
      } else if (
        this.answered &&
        this.selected === option &&
        this.currentQuestion.correct_answer !== option
      ) {
        answerClass = "incorrect";
      }
      return answerClass;
    },
  },

  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selected = null;
        this.answered = false;
        this.shuffleAnswers(this.options);
      },
    },
  },
  computed: {
    options() {
      let options = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      return options;
    },
  },
};
</script>
<style lang="css" scoped>
.tile.is-child {
  margin: 0.6rem 0 !important;
}
.buttons {
  justify-content: center !important;
}
.answer {
  cursor: pointer;
  transition: all 0.7s;
}
.answer p {
  color: white !important;
}
.answer:hover,
.answer:focus {
  background-color: #0f0f0f !important;
  transition: all 0.7s;
}
.selected {
  background-color: hsl(0, 0%, 4%) !important;
}
.correct {
  background-color: hsl(141, 71%, 48%) !important;
}
.incorrect {
  background-color: hsl(348, 100%, 61%) !important;
}
</style>