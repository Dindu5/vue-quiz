<template>
  <section class="hero is-medium is-dark is-bold main">
    <div class="hero-body">
      <div class="container">
        <Header :submitted="submitted" :correctAnswers="correctAnswers" />
        <QuestionBox
          v-if="questions.length"
          :currentQuestion="questions[index]"
          :next="next"
          :increment="increment"
        />
      </div>
    </div>
  </section>
</template>

<script>
import QuestionBox from "./components/QuestionBox.vue";
import Header from "./components/Header.vue";

export default {
  name: "App",
  components: {
    QuestionBox,
    Header,
  },
  data() {
    return {
      questions: [],
      index: 0,
      submitted: 0,
      correctAnswers: 0,
    };
  },
  methods: {
    next() {
      if (this.index < 20) {
        this.index++;
      }
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.correctAnswers++;
      }
      this.submitted++;
    },
  },
  mounted: function () {
    fetch("https://opentdb.com/api.php?amount=20&category=17", {
      method: "get",
    })
      .then((res) => {
        return res.json();
      })
      .then((jsonData) => {
        this.questions = jsonData.results;
      });
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
body {
  color: #2c3e50;
}
.main {
  min-height: 100vh;
}
</style>
