<template>
  <Quiz :questions="questions"/>
</template>

<script>
import fetch from "isomorphic-fetch";
import Quiz from "~/components/Quiz.vue";

export default {
  components: {
    Quiz
  },
  async asyncData() {
    const response = await fetch("https://api.myjson.com/bins/a4bei");
    const questions = await response.json();
    return {
      questions: questions.questions
    };
  }
};
</script>

<style>
</style>
