<template>
  <div class="question-container">
    <div class="counter-points-container">
      <h2>Question {{ questionCounter }}</h2>
      <h2>{{ points }} / {{ numberOfQuestions }}</h2>
    </div>
    <h2 class="the-question">{{ currentQuestion.questionItself }}</h2>
    <form class="form-answers">
      <div
        v-for="(answer,index) in currentQuestion.answers"
        v-bind:key="index"
        class="one-answer-container"
      >
        <input
          type="radio"
          name="quiz"
          v-bind:id="answer.id"
          v-bind:value="answer.id"
          v-model="picked"
        />
        <label
          v-bind:for="answer.id"
          :class="[answer.id === picked ? 'selected' : '']"
        >{{ answer.answer }}</label>
        <br />
      </div>
    </form>
    <button v-show="!isLastQuestion" v-on:click="showNextQuestion">NEXT</button>
    <button v-show="isLastQuestion" v-on:click="finishQuiz">FINISH QUIZ</button>
  </div>
</template>

<script>
export default {
  name: "question",
  props: {
    currentQuestion: {
      type: Object
    },
    questionCounter: {
      type: Number
    },
    points: {
      type: Number
    },
    isLastQuestion: {
      type: Boolean
    },
    numberOfQuestions: {
      type: Number
    }
  },
  data() {
    return {
      picked: ""
    };
  },
  methods: {
    showNextQuestion: function() {
      this.$emit("showNextQuestion", this.picked);
      this.picked = "";
    },
    finishQuiz: function() {
      this.$emit("finishQuiz", this.picked);
      this.picked = "";
    }
  }
};
</script>

<style scoped>
.question-container {
  color: #4f3a65;
  text-align: center;
}

.counter-points-container {
  display: flex;
  justify-content: space-around;
  align-items: center;
  margin-bottom: 30px;
}

.the-question {
  max-width: 800px;
  margin-bottom: 10px;
}

.form-answers {
  min-height: 300px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.one-answer-container {
  background-color: #fdef96;
  padding: 20px;
  width: 550px;
  margin: 5px 0px;
  border-radius: 10px;
}
.one-answer-container:hover {
  padding: 20px;
  background-color: #b2d3be;
  cursor: pointer;
}

input {
  visibility: hidden;
}

label {
  cursor: pointer;
}

.selected {
  padding: 20px;
  background-color: #39bdc8;
  color: #fcf9ea;
}

button {
  border-radius: 10px;
  padding: 15px 25px;
  background: #95adbe;
  color: #fcf9ea;
  border: none;
  font-size: 25px;
}

button:hover {
  background-color: #fdef96;
  color: #4f3a65;
  cursor: pointer;
}
</style>
