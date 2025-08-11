<template>
  <div class="app">
    <h1>Vue.js Quiz App</h1>

    <div v-if="!quizCompleted">
      <h2>{{ currentQuestion.question }}</h2>

      <ul>
        <li 
          v-for="(answer, index) in currentQuestion.answers" 
          :key="index"
          :class="{ selected: selectedAnswer === index, correct: showAnswers && index === currentQuestion.correct, wrong: showAnswers && index === selectedAnswer && index !== currentQuestion.correct }"
          @click="selectAnswer(index)"
        >
          {{ answer }}
        </li>
      </ul>

      <button @click="nextQuestion" :disabled="selectedAnswer === null">
        {{ currentQuestionIndex === questions.length - 1 ? 'Finish' : 'Next' }}
      </button>
    </div>

    <div v-else>
      <h2>Quiz Completed!</h2>
      <p>Your Score: {{ score }} / {{ questions.length }}</p>
      <button @click="restartQuiz">Restart</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentQuestionIndex: 0,
      selectedAnswer: null,
      showAnswers: false,
      score: 0,
      quizCompleted: false,
      questions: [
        {
          question: "What is the colour of the sun?",
          answers: ["Blue", "Green", "Yellow", "Magenta"],
          correct: 2
        },
        {
          question: "Who created Vue.js?",
          answers: ["Brendan Eich", "Evan You", "Ryan Dahl", "Linus Torvalds"],
          correct: 1
        },
        {
          question: "Which directive is used for conditional rendering in Vue?",
          answers: ["v-if", "v-for", "v-bind", "v-model"],
          correct: 0
        }
      ]
    };
  },
  computed: {
    currentQuestion() {
      return this.questions[this.currentQuestionIndex];
    }
  },
  methods: {
    selectAnswer(index) {
      if (!this.showAnswers) {
        this.selectedAnswer = index;
        this.showAnswers = true;
        if (index === this.currentQuestion.correct) {
          this.score++;
        }
      }
    },
    nextQuestion() {
      this.showAnswers = false;
      this.selectedAnswer = null;
      if (this.currentQuestionIndex < this.questions.length - 1) {
        this.currentQuestionIndex++;
      } else {
        this.quizCompleted = true;
      }
    },
    restartQuiz() {
      this.currentQuestionIndex = 0;
      this.selectedAnswer = null;
      this.showAnswers = false;
      this.score = 0;
      this.quizCompleted = false;
    }
  }
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
}
.app {
  max-width: 500px;
  margin: auto;
  text-align: center;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  background: #eee;
  padding: 10px;
  margin: 5px 0;
  cursor: pointer;
  border-radius: 5px;
}
li.selected {
  background: #d0ebff;
}
li.correct {
  background: #b2f2bb;
}
li.wrong {
  background: #ffa8a8;
}
button {
  padding: 10px 20px;
  margin-top: 10px;
}
</style>
