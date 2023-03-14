<template>
    <div class="stepper-container">
      <div class="stepper">
        <div class="stepper-progress">
          <div class="stepper-progress-bar" :style="'width:' + stepperProgress">
  
          </div>
        </div>
        <div class="stepper-item" :class="{'current': step == item, 'success': step > item}" v-for="item in 3" :key="item">
          <div class="stepper-item-counter">
            <img class="icon-success" src="@/assets/images/checked.png" alt="">
            <span class="number">
              {{ item }}
            </span>
          </div>
          <span class="stepper-item-title">
            Step {{ item }}
          </span>
        </div>
      </div>
      <div class="stepper-content">
        <div class="stepper-sheet" v-if="step === 1">
          <p>Please enter your name:</p>
          <input type="text" v-model="name" />
        </div>
        <div class="stepper-sheet" v-else-if="step === 2">
          <p>Here are the rules:</p>
          <ul>
            <li>You can't pass to another question without answering</li>
            <li>In the end of the quiz you'll get the feedback containing the wrong answers and their correction</li>
            <li>You can redo the quiz at the end</li>
          </ul>
        </div>
        <div class="stepper-sheet" v-else-if="step === 3">
          <p>Are you ready to start the quiz?</p>
          <button class="btn btn-purple-1 startBtn" @click="$emit('start-quiz')">Start Quiz</button>
        </div>
        <!-- <div class="stepper-sheet" v-else-if="step === 4">
          <p>Congratulations! You have completed the quiz.</p>
        </div> -->
      </div>
      <div class="controls">
        <button class="btn" @click="step--" :disabled="step === 1">
          Back
        </button>
        <button class="btn btn-purple-1" @click="nextStep" :disabled="step === 3 || (step === 1 && name.length === 0)">
          Next
        </button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data: () => ({
      step: 1,
      name: '',
    }),
    computed: {
      stepperProgress() {
        return (100 / 2) * (this.step - 1) + '%'
      },
      isNameEntered() {
        return this.name.length > 0
      },
    },
    methods: {
      nextStep() {
        if (this.step < 3) {
          this.step++
        }
      },
    },
  }
  </script>
  
  
  
<style>
li{
  list-style:decimal;
  text-align: left;
}
.stepper-container{
    background-color: #fff;
    min-width: 600px;
    padding: 60px;
    border-radius: 32px;
    box-shadow: rgba(#000, #000, #000, 0.9);
}
.stepper{
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
    position: relative;
    z-index: 0;
    margin-bottom: 24px;
}
.stepper .stepper-progress{
    position: absolute;
    background-color: #D3D3D3;
    height: 2px;
    z-index: -1;
    left: 0;
    right: 0;
    margin-top: 0 auto;
}
.stepper-progress .stepper-progress-bar{
    position: absolute;
    height: 100%;
    width: 0%;
    background-color: #ab3cfc;
    transition: all 500ms linear;
}
.stepper-item{
    display: flex;
    flex-direction: column;
    align-items: center;
    color: #D3D3D3;
    transition: all 500ms linear;
}

.stepper-item-counter{
    height: 68px;
    width: 68px;
    display: grid;
    place-items: center;
    background-color: #fff;
    border-radius: 100%;
    border: 2px solid #D3D3D3;
    position: relative;
}
.stepper-item-counter .icon-success{
    position: absolute;
    opacity: 0;
    transform: scale(0);
    width: 24px;
    transition: all 500ms linear;
}

.stepper-item-counter .number{
    font-size: 22px;
    transition: all 500ms linear;
}

.stepper-item .stepper-item-title{
    font-size: 14px;
    position: absolute;
    bottom: -24px;
    color: #D3D3D3;
}

.stepper-item.success .stepper-item-counter{
    border-color: #ab3cfc;
    background-color: #533b7c;
    color: #fff;
    font-weight: 600;
}

.stepper-item.success .stepper-item-counter .icon-success{
    opacity: 1;
    transform: scale(1);
}
.stepper-item.success .stepper-item-counter .number{
    opacity: 0;
    transform: scale(0);
}

.stepper-item.success .stepper-item-title{
    color: #533b7c;
}

.stepper-item.current .stepper-item-counter{
    border-color: #533b7c;
    background-color: #533b7c;
    color: #fff;
    font-weight: 600;
}

.stepper-item.current .stepper-item-title{
    color: #D3D3D3 ;
}

.stepper-sheet{
    color: #3e394c;
    text-align: center;
    padding: 80px 40px;
    margin-top: 50px;
    box-shadow: 0px -1px 35px 1px rgba(0,0,0,0.43);
-webkit-box-shadow: 0px -1px 35px 1px rgba(0,0,0,0.43);
-moz-box-shadow: 0px -1px 35px 1px rgba(0,0,0,0.43);;
}

.controls{
    display: flex;
}
 .btn{
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 6px 16px;
    border: 1px solid;
    text-align: center;
    vertical-align: middle;
    cursor: pointer;
    line-height: 1.6;
    transition: all 150ms;
    border-radius: 4px;
    width: fit-content;
    font-size: 0.75rem;
    color: #3e394c;
    background-color: red;
    border-color: #D3D3D3;
    margin-top: 20px;
 }

 .btn:disabled{
    opacity: 0.5;
    pointer-events: none;
 }

 .btn.btn-purple-1{
    background-color: #ab3cfc;
    border-color: #ab3cfc;
    color: #fff;
    margin-left: auto;
 }
</style>