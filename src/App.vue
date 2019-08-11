<template>
  <div id="app">
    <app-header></app-header>
    <router-view></router-view>
  </div>
</template>

<script>
import AppHeader from "./components/Header";
import Quiz from "./components/Quiz.vue";
import Question from "./components/Question.vue";
import AddQuestion from "./components/AddQuestion.vue";

export default {
  name: "app",
  components: {
    quiz: Quiz,
    question: Question,
    "add-question": AddQuestion,
    "app-header": AppHeader
  },
  data() {
    return {

    }
  },
  methods: {
    
  }
};
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  html, body {
    height: 100vh;
    width: 100vw;
    overflow-x: hidden;
  }

  body {
    background-image: url('./assets/question-marks-bcg.jpg');
  }

  #app {
    background-color: rgba(223,240,234, 0.9);
    width: 75%;
    height: 100%;
    margin: 0 auto;
    padding-top: 10px;
    border-radius: 50px;
  }
</style>
