<template>
  <div id="app">
    <link href="https://fonts.googleapis.com/css?family=Montserrat&display=swap" rel="stylesheet">
    <Header :numCorrect="numCorrect" :numTotal="numTotal"/>
    <div v-if="questions.length != 0" class="question-box-container">
      <b-container class="bv-example-row">
        <b-row>
          <b-col sm="6" offset="3 ">
            <QuestionBox :currentQuestion="questions[index]" :increment="increment" :next="incrementQuestionCounter"/>
          </b-col>
        </b-row>
      </b-container>
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'


export default {
  name: 'app',
  components: {
    Header,
    QuestionBox,
  },
  data(){
    return {
      questions:[],
      index : 0,
      numCorrect : 0,
      numTotal: 0,
    }
  },
  methods:{
    incrementQuestionCounter(){
      if(this.index !== this.questions.length){
        this.index++;
      }
    },
    increment(isCorrect){
      if(isCorrect){
        this.numCorrect++
      }
      this.numTotal++
    }
  },
  mounted:function(){
    fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple',{
      method:'get'
    })
    .then((response) => {
      return(response.json())
    })
    .then((jsonData) =>{
      this.questions = jsonData.results; 
    })
  }
}
</script>

<style>
#app {
  font-family: 'Montserrat', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.question-box-container{
  margin-top: 20px;
}
</style>
