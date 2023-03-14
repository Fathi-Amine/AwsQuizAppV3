<script>
  export default {
    props: {
      totalQuestions: {
        type: Number,
        required: true
      },
      currentQuestionIndex: {
        type: Number,
        required: true
      },
      quizCompleted: {
        type: Boolean,
        default: false
      }
    },
    computed: {
      percentage() {
        if (this.quizCompleted) {
          return '100%';
        } else {
            return ((this.currentQuestionIndex + 1) / this.totalQuestions) * 100 + '%';
        }
      }
    }
  };
</script>

<template>
  <div v-if="!quizCompleted" class="progress-bar">
    <div class="progress-bar-inner" :style="{ width: percentage }"></div>
  </div>
</template>

<style>
  .progress-bar {
    background-color: #e0e0de;
    height: 30px;
    width: 100%;
    border-radius: 5px;
    overflow: hidden;
  }

  .progress-bar-inner {
    background-color: #ab3cfc;
    height: 100%;
    width: 0;
    transition: width 0.5s ease;
  }
</style>
