<template>
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

      <button :disabled="selectedChoiceIndex === null || answered" @click="checkAnswer">Submit</button>
      <button :disabled="selectedChoiceIndex === null || !answered" @click="nextItem">Next</button>
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
      userWrongAnswers: []
    };
  },
  computed: {
    shuffledQuestions() {
      // Shuffle the questions array randomly
      return this.shuffleArray(this.questions)[this.index];
    },
    correctAnswer() {
      return this.shuffledQuestions.choices.find(choice => choice.isCorrect);
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
        const correctIndex = this.shuffledQuestions.choices.findIndex(choice => choice.isCorrect);
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
                    id: this.shuffledQuestions.id,
                    answer: this.shuffledQuestions.choices[this.selectedChoiceIndex].answer
                });
                }
            this.userAnswers.push({
            id: this.shuffledQuestions.id,
            answer: this.shuffledQuestions.choices[this.selectedChoiceIndex].answer
            });
            console.log(this.userAnswers);
            console.log(this.userWrongAnswers);
        }
    }
  }
};
</script>

<style>
.selected {
  background-color: yellow;
}
.correct {
  background-color: green;
}
.incorrect {
  background-color: red;
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
  