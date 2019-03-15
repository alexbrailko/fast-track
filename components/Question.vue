<template>
  <div class="container">
    <div class="box">
      <h2>Question {{ questionNumber }}:</h2>

      <h3>{{ question.text }}</h3>

      <form v-if="question.type === 'mc'" v-on:submit.prevent>
        <div v-for="(mcanswer,index) in question.answers" v-bind:key="index">
          <input
            type="radio"
            :id="'answer'+index+questionNumber"
            name="currentQuestion"
            v-model="answer"
            :value="mcanswer"
            @click="submitAnswer"
          >
          <label :for="'answer'+index+questionNumber">{{mcanswer}}</label>
          <br>
        </div>
        <button class="next" @click="goToNextSlide">Next</button>
        <div class="error" v-if="error">Please select an answer</div>
      </form>
    </div>
  </div>
</template>

<script>
import VueScrollTo from "vue-scrollto";

export default {
  name: "Question",
  data() {
    return {
      answer: "",
      error: false
    };
  },
  props: ["question", "question-number"],

  methods: {
    submitAnswer: function(e) {
      this.error = false;
      this.$emit("answer", {
        questionNumber: this.questionNumber,
        answer: e.target.value
      });
    },
    goToNextSlide: function() {
      if (!this.answer) {
        this.error = true;
      } else {
        if (this.questionNumber === 5) {
          VueScrollTo.scrollTo("#results");
        } else {
          VueScrollTo.scrollTo("#id" + this.questionNumber);
        }
      }
    }
  }
};
</script>

<style scoped>
h2,
h3 {
  margin-top: 0;
  margin-bottom: 15px;
}
.box {
  background: rgba(0, 0, 0, 0.8);
  padding: 40px;
  color: white;
  text-align: left;
}

form > div {
  margin-bottom: 10px;
}

form input,
form label {
  cursor: pointer;
}

.next {
  margin-top: 20px;
  border: none;
  font-size: 16px;
  background: white;
  padding: 5px 15px;
  cursor: pointer;
}

.error {
  color: red;
}

@media all and (min-width: 800px) {
  .box {
    min-width: 600px;
  }
}
</style>