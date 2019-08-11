<template>
  <div>
    <h3 v-show="notification">Your question was successfully saved!</h3>
    <form>
      <label for="question">What is the question?</label>
      <br />
      <textarea type="text" id="question" v-model="inputQuestion.questionItself" required></textarea>
      <br />
      <label for="corrAns">What is the correct answer?</label>
      <br />
      <textarea type="text" id="corrAns" v-model="inputQuestion.answers[0].answer" required></textarea>
      <br />
      <label>What are the other possible, wrong answers?</label>
      <br />
      <textarea type="text" id="wrongAns1" v-model="inputQuestion.answers[1].answer" required></textarea>
      <br />
      <textarea type="text" id="wrongAns2" v-model="inputQuestion.answers[2].answer"></textarea>
      <br />
      <textarea type="text" id="wrongAns3" v-model="inputQuestion.answers[3].answer"></textarea>
      <br />
      <textarea type="text" id="wrongAns4" v-model="inputQuestion.answers[4].answer"></textarea>
      <br />

      <label for="score">How many scores is this question worth? (1 or 3)</label>
      <br />
      <input type="number" v-model="inputQuestion.score" required />
      <br />
      <button v-on:click.prevent="addNewQuestion">add this question to the quiz</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "add-question",
  data() {
    return {
      notification: false,
      inputQuestion: {
        questionItself: "",
        picture: "",
        score: null,
        hasBeenShowed: false,
        answers: [
          { id: 0, answer: "" },
          { id: 1, answer: "" },
          { id: 2, answer: "" },
          { id: 3, answer: "" },
          { id: 4, answer: "" }
        ]
      }
    };
  },
  methods: {
    addNewQuestion: function() {
      var self = this;

      //new object is needed because the answers with empty string cannot be sent
      //otherwise empty strings would be shown in the quiz as answer options
      let objectToBeSent = {
        questionItself: this.inputQuestion.questionItself,
        picture: this.inputQuestion.picture,
        score: this.inputQuestion.score,
        hasBeenShowed: this.inputQuestion.hasBeenShowed,
        answers: self.omitEmptyStringAnswers()
      };

      axios
        .post(
          "https://kresz-kviz.firebaseio.com/questions.json",
          objectToBeSent
        )
        .then(data => {
          if (data.status === 200) {
            self.notification = true;
            self.clearInputFields();
          }
        })
        .catch(error => {
          console.log(error);
        });
      setTimeout(function() {
        self.notification = false;
      }, 9000);
    },
    clearInputFields: function() {
      this.inputQuestion.questionItself = "";
      this.inputQuestion.picture = "";
      this.inputQuestion.score = null;
      this.inputQuestion.hasBeenShowed = false;

      for (let i = 0; i < this.inputQuestion.answers.length; i++) {
        this.inputQuestion.answers[i].answer = "";
      }
    },
    omitEmptyStringAnswers: function() {
      let newAnswersArray = [];
      for (let i = 0; i < 5; i++) {
        if (this.inputQuestion.answers[i].answer !== "") {
          newAnswersArray.push(this.inputQuestion.answers[i]);
        }
      }
      return newAnswersArray;
    }
  }
};
</script>

<style scoped>
form {
  color: #4f3a65;
  text-align: center;
  font-size: 22px;
}

textarea {
  resize: none;
  width: 500px;
  margin-bottom: 10px;
  font-size: 18px;
  border: #b2d3be 0.5px solid;
  color: #4f3a65;
  outline-color: #fdef96;
}

input {
  border: #b2d3be 0.5px solid;
  width: 40px;
  height: 25px;
  color: #4f3a65;
  outline-color: #fdef96;
}

button {
  border-radius: 10px;
  padding: 15px 25px;
  background: #95adbe;
  color: #fcf9ea;
  border: none;
  font-size: 25px;
  margin-top: 20px;
}

button:hover {
    background-color: #fdef96;
    color: #4f3a65;
    cursor: pointer;
  }
</style>
