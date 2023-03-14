<template>
  <div class="container">
    <div v-if="showQuiz" class="quiz">
      <div class="progress">
        <progress-bar :total-questions="questions.length" :current-question-index="index"></progress-bar>
      </div>

      <h2>{{ currentQuestion.question }}</h2>
      <div>
        <button
        class="choiceBtn"
          v-for="(choice, choiceIndex) in currentQuestion.choices"
          :key="choiceIndex"
          :class="{ 
            selected: isChoiceSelected(choiceIndex), 
            correct: isChoiceCorrect(choiceIndex) && answered, 
            incorrect: selectedChoiceIndex === choiceIndex && answered && !isChoiceCorrect(choiceIndex) 
          }"
          @click="highlightSelected(choiceIndex)"
        >
          {{ choice.answer }}
        </button>

      </div>

      <div class="btns-wrapper">
        <button class="navBtn" :disabled="selectedChoiceIndex === null || answered" @click="checkAnswer">Submit</button>
        <button class="navBtn" v-if="showNextButton" :disabled="selectedChoiceIndex === null || !answered" @click="nextItem">Next</button>
        <button class="navBtn" v-if="showResultsButton" @click="showResults">Show Results</button>
      </div>
    </div>
    <div  v-else class="results-container">
      <h2>Results</h2>
      <ul>
    <li class="feedback" v-for="wrongAnswer in wrongAnswers" :key="wrongAnswer.question">
      <p>{{ wrongAnswer.question }}</p>
      <p class="userAnswer">Your answer: {{ wrongAnswer.userAnswer }}</p>
      <p class="correctAnswer">Correct answer: {{ wrongAnswer.correctAnswer }}</p>
      <p class="desc">Feedback: {{ wrongAnswer.feedback }}</p>
    </li>
  </ul>
  <button class="navBtn restartBtn" @click="restartQuiz">Restart</button>
    </div>
  </div>
</template>

<script>
import ProgressBar from '@/components/ProgressBarComponent.vue';

export default {
  components: {
    ProgressBar
  },
  props: {
    questions: Array,
    required: true
  },
  data() {
    return {
      index: 0,
      selectedChoiceIndex: null,
      answered: false,
      correct: false,
      userAnswers: [],
      userWrongAnswers: [],
      showResultsButton: false,
      shuffledQuestions: this.shuffleArray(this.questions),
      showQuiz: true,
      wrongAnswers: []
    };
  },
  computed: {
    currentQuestion() {
      return this.shuffledQuestions[this.index];
    },
    correctAnswer() {
      return this.currentQuestion.choices.find(choice => choice.isCorrect);
    },
    showNextButton() {
      return this.index !== this.questions.length - 1;
    }
  },
  methods: {
    nextItem() {
      if (this.index < this.questions.length - 1) {
        this.index++;
      } else {
        this.index = 0;
      }
      this.selectedChoiceIndex = null; // Reset selected choice index
      this.answered = false; // Reset answered flag
    },
    shuffleArray(array) {
      // Shuffle the array randomly
      return array.sort(() => Math.random() - 0.5);
    },
    isChoiceSelected(index) {
      return this.selectedChoiceIndex === index;
    },
    highlightSelected(index) {
      if (!this.answered) {
        this.selectedChoiceIndex = index;
      }
    },
    isChoiceCorrect(index) {
      if (this.answered) {
        const correctIndex = this.currentQuestion.choices.findIndex(choice => choice.isCorrect);
        return index === correctIndex;
      } else {
        return false;
      }
    },
    checkAnswer() {
      if (this.selectedChoiceIndex !== null) {
        this.answered = true;
        this.correct = this.isChoiceCorrect(this.selectedChoiceIndex);
        if (!this.correct) {
          this.userWrongAnswers.push({
            id: this.currentQuestion.id,
            answer: this.currentQuestion.choices[this.selectedChoiceIndex].answer
          });
        }
        this.userAnswers.push({
          id: this.currentQuestion.id,
          answer: this.currentQuestion.choices[this.selectedChoiceIndex].answer
        });
        console.log(this.userAnswers);
        console.log(this.userWrongAnswers);
        if (this.index === this.questions.length - 1) {
          this.showResultsButton = true;
        }
      }
    },
    showResults() {
      this.wrongAnswers = [];

this.userWrongAnswers.forEach(userAnswer => {
  const question = this.questions.find(question => question.id === userAnswer.id);
  const correctAnswer = question.choices.find(choice => choice.isCorrect).answer;

  this.wrongAnswers.push({
    question: question.question,
    userAnswer: userAnswer.answer,
    correctAnswer: correctAnswer,
    feedback: question.description
  });
});

this.showQuiz = false;
},
restartQuiz() {
      this.index = 0;
      this.selectedChoiceIndex = null;
      this.answered = false;
      this.correct = false;
      this.userAnswers = [];
      this.userWrongAnswers = [];
      this.showQuiz = false;
      this.showResultsButton = false;
      this.wrongAnswers = [];
      this.shuffledQuestions = this.shuffleArray(this.questions);
      location.reload();
    }
  }
};
</script>


<style>
.container{
  width: 500px;
  background-color: #533b7c;
  padding: 2rem 1rem ;
  border-radius: 20px;
}
.selected {
  background-color: yellow;
}
.correct {
  background-color: green;
}
.incorrect {
  background-color: red;
}
.progress{
  width: 200px;
}

.btns-wrapper{
  display: flex;
  justify-content:flex-start;
  align-items: center;
  margin-top: 2rem;
  gap: 1rem;
}
.choiceBtn{
  display: block;
  background-color: #7e6df3;
  font-size: 1.25rem;
  font-weight: 600;
  width: 100%;
  height: 3rem;
  margin-top: 1rem;
  border: none;
  border-radius: 12px;
  color: #fff;
  cursor:pointer;
}

.choiceBtn:hover{
  opacity: 0.8;
}

.navBtn{
  width: 6rem;
  height: 3rem;
  font-size:1rem;
  font-weight: 700;
  background-color: #3e394c;
  color: #fff;
  border-radius: 12px;
  border: none;
  cursor:pointer;
}

.navBtn:disabled{
  opacity: 0.5;
}
.selected {
  background-color: yellow;
  color: black;
}
.correct {
  background-color: green;
}
.incorrect {
  background-color: red;
}

.feedback{
  background-color: #9f8cca;
  padding: 1rem;
}

.feedback .userAnswer{
  background-color: red;
  padding: 0.75rem;
  border-radius: 8px;
}
.feedback .correctAnswer{
  background-color: green;
  padding: 0.75rem;
  border-radius: 8px;
}

.feedback .desc{
  background-color: #7e6df3;
  padding: 0.75rem;
  border-radius: 8px;
}

.restartBtn {
  display: block;
  margin: 0 auto;
  cursor:pointer;
}
</style>

<!-- <template>
    <div class="container">
      <div>
        <progress-bar :total-questions="questions.length" :current-question-index="index"></progress-bar>
  
        <h2>{{ shuffledQuestions.question }}</h2>
        <div>
          <button
            v-for="(choice, choiceIndex) in shuffledQuestions.choices"
            :key="choiceIndex"
            :class="{ 
              selected: isChoiceSelected(choiceIndex), 
              correct: isChoiceCorrect(choiceIndex) && answered, 
              incorrect: selectedChoiceIndex === choiceIndex && answered && !isChoiceCorrect(choiceIndex) 
            }"
            @click="highlightSelected(choiceIndex)"
          >
            {{ choice.answer }}
          </button>
  
        </div>
        
        <button v-if="!showResults" :disabled="selectedChoiceIndex === null || answered" @click="checkAnswer">Submit</button>
        <button v-if="!showResults" :disabled="selectedChoiceIndex === null || !answered" @click="nextItem">Next</button>
        <button v-if="showResults" @click="resetQuiz">Reset Quiz</button>
        <button v-if="!showResults" @click="showResults = true">Show Results</button>
        <div v-if="showResults">
          <h3>Your Answers</h3>
          <ul>
            <li v-for="(answer, index) in userAnswers" :key="index">{{ answer }}</li>
          </ul>
        </div>
      </div>
    </div>
  </template> -->
  

  <!-- methods: {
    // Existing methods here
    resetQuiz() {
      this.index = 0;
      this.selectedChoiceIndex = null;
      this.answered = false;
      this.correct = false;
      this.userAnswers = [];
      this.showResults = false;
    }
  } -->
  