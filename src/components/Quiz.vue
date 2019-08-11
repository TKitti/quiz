<template>
    <div class="main-view">
      <button v-on:click="startQuiz" v-if="isStartButtonShown" class="start-button">start the quiz now</button>
      <question
        v-show="areQuestionsShown"
        v-bind:currentQuestion="currentQuestion"
        v-bind:questionCounter="questionCounter"
        v-bind:points="points"
        v-bind:isLastQuestion="isLastQuestion"
        v-bind:numberOfQuestions="numberOfQuestions"
        v-on:showNextQuestion="getNextQuestion($event)"
        v-on:finishQuiz="getResult($event)"
      ></question>
      <div v-if="resultShown">
        <p>Your score is: {{ points }} / {{ questions.length }}</p>
        <button v-on:click="quizAgain">take quiz again</button>
      </div>
    </div>
</template>

<script>
import Question from "./Question";
import axios from "axios";

export default {
  name: "quiz",
  components: {
    question: Question
  },
  data() {
    return {
      areQuestionsShown: false,
      isStartButtonShown: true,
      resultShown: false,
      questions: [],
      currentQuestion: {},
      currentIndex: 0,
      questionCounter: 1,
      points: 0,
      pickedAnswer: "",
      isLastQuestion: false,
      numberOfQuestions: 0
    };
  },
  methods: {
    startQuiz: function() {
      this.shuffle(this.questions);
      this.currentQuestion = this.questions[0];
      this.shuffle(this.currentQuestion.answers);
      this.areQuestionsShown = true;
      this.isStartButtonShown = false;
      this.numberOfQuestions = this.questions.length;
    },
    getNextQuestion: function(picked) {
      this.pickedAnswer = picked;
      this.checkAnswer();
      this.currentQuestion = this.questions[++this.currentIndex];
      this.shuffle(this.currentQuestion.answers);
      this.questionCounter++;
      this.checkIfLastQuestion();
    },
    shuffle: function(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
    },
    checkAnswer: function() {
      if (this.pickedAnswer === 0) {
        this.points++;
      }
    },
    checkIfLastQuestion: function() {
      if (this.questionCounter === this.questions.length) {
        this.isLastQuestion = true;
      }
    },
    getResult: function(picked) {
      this.pickedAnswer = picked;
      this.checkAnswer();
      this.questionCounter = 1;
      this.currentIndex = 0;
      this.areQuestionsShown = false;
      this.resultShown = true;
    },
    quizAgain: function() {
      this.resultShown = false;
      this.isStartButtonShown = true;
      this.points = 0;
      this.isLastQuestion = false;
    }
  },
  created() {
    axios
      .get("https://kresz-kviz.firebaseio.com/questions.json")
      .then(data => {
        //converting the object of objects (received from server) into an array of objects
        this.questions = Object.keys(data.data).map(i => data.data[i]);
      })
      .catch(error => {
        console.log(error);
      });
  }
};
</script>

<style scoped>
  .main-view {
    width: 100%;
    margin: 20px auto;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .start-button {
    background-color: #95adbe;
    color: #fcf9ea;
    border: none;
    margin-top: 80px;
    padding: 30px;
    font-size: 50px;
    border-radius: 10px;
    cursor: pointer;
    text-transform: uppercase;
  }
</style>
