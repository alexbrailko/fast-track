<template>
  <div class="fullscreen">
    <div v-if="introStage" class="intro container">
      <h1>Welcome to the Quiz</h1>
      <button @click="scrollToQuiz">START!</button>
    </div>

    <div
      v-for="(question, index) in questions"
      v-bind:key="index"
      :id="'id' + index"
      class="question fullscreen"
    >
      <Question :question="questions[index]" v-on:answer="handleAnswer" :question-number="index+1"/>
    </div>

    <div class="fullscreen" id="results" v-if="resultsStage">
      <div class="container results">
        <button v-if="!showResults" @click="handleResults">Get results</button>
        <p
          v-if="showResults"
        >You got {{correct}} right out of {{questions.length}} questions. Your percentage is {{perc}}%.</p>
      </div>
    </div>
  </div>
</template>

<script>
import VueScrollTo from "vue-scrollto";

import Question from "./Question";

export default {
  name: "Quiz",
  components: {
    Question
  },
  props: ["questions"],
  data() {
    return {
      introStage: false,
      resultsStage: false,
      showResults: false,
      answers: [],
      correct: 0,
      perc: null
    };
  },

  created() {
    if (this.questions) {
      this.introStage = true;
    }
  },
  methods: {
    scrollToQuiz() {
      VueScrollTo.scrollTo("#id0");
    },
    handleAnswer(e) {
      this.answers[e.questionNumber - 1] = e.answer;
      if (this.answers.length === 5) {
        this.resultsStage = true;
      }
    },
    handleResults() {
      this.questions.forEach((a, index) => {
        if (this.answers[index] === a.answer) this.correct++;
      });
      this.perc = (this.correct / this.questions.length) * 100;
      this.showResults = true;
    }
  }
};
</script>

 
<style scoped>
.fullscreen {
  display: block;
  position: relative;
  min-width: 100%;
  min-height: 100%;
  height: 100vh;
}
.container {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
h1 {
  margin-top: 0;
}
button {
  background: black;
  border: none;
  color: #fff;
  padding: 10px 20px;
  cursor: pointer;
}
.intro {
  background: url("../assets/images/intro.jpg") no-repeat center center fixed;
  background-size: cover;
}
.question {
  background-size: cover;
  background-repeat: no-repeat;
  background-attachment: fixed;
}

.question#id0 {
  background-image: url("../assets/images/0.jpg");
}
.question#id1 {
  background-image: url("../assets/images/1.jpg");
}
.question#id2 {
  background-image: url("../assets/images/2.jpg");
}
.question#id3 {
  background-image: url("../assets/images/3.jpg");
}
.question#id4 {
  background-image: url("../assets/images/4.jpg");
}

.results {
  font-size: 35px;
}

.results p {
  padding: 0 15px;
}
</style>
