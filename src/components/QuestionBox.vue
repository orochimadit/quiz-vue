<template>
  <div class="question-box-container">
    <b-jumbotron >

    <template slot="lead">
     {{currentQuestion.question}}
    </template>

    <hr class="my-4">


<b-list-group>
  <b-list-group-item 
  v-for="(answer,index) in answers" 
  :key="index"
  @click="selectAnswer(index)"
  :class="answerClass(index)"

  >   {{answer}}
  </b-list-group-item>
  
  
</b-list-group>
   
    <b-button variant="primary" href="#"
      @click  ="submitAnswer"
      :disabled="selectedIndex===null || answered"
    >Submit</b-button>
    <b-button @click="next" variant="success" href="#">Next</b-button>
  </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  props:{
    currentQuestion:Object,
    next:Function,
    increment:Function
  },
  data(){
    return{
      selectedIndex:null,
      correct_index:null,
      shuffledAnswers:[],
      answered:false,
    }
  },
   watch:{
    currentQuestion:{
      immediate:true,
      handler(){
      this.selectedIndex=null
      this.answered=false
      this.shuffleAnswers()
      }
    }
  },
  methods:{
    selectAnswer(index){
      this.selectedIndex=index  
    },shuffleAnswers(){
      let answers=[...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
      this.shuffledAnswers=_.shuffle(answers)
      this.correct_index = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },submitAnswer(){
      let isCorrect=false
        
      if(this.selectedIndex===this.correct_index){
        isCorrect=true
      }
      this.answered=true
      this.increment(isCorrect)
    },
    answerClass(index){
      let answerClass=''
      if( !this.answered && this.selectedIndex===index){
        answerClass='selected'
      }else if(this.answered && this.correct_index===index){
        answerClass='correct'
      }else if(  this.answered && this.selected_index===index && this.correct_index!== index){
        answerClass='incorrect'
      }
      return answerClass
      
    }
  },
  computed:{
    answers(){
      let answers = [...this.currentQuestion.incorrect_answers]
    answers.push(this.currentQuestion.correct_answer)
     return answers
    }
  },
 
}
</script>
<style scoped>
  .list-group{
    margin-bottom: 15px;
  }
  .btn{
    margin: 0 5px;
  }
  .list-group-item:hover{
    background: #eee;
    cursor: pointer;
  }
  .selected{
    background-color: lightblue;
  }
  .correct{
    background-color: lightgreen;
  }
  .incorrect{
    background-color: red;
  }
</style>
