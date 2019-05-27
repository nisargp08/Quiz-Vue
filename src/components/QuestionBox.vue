<template>
    <div>
        <b-jumbotron>
            <template slot="header"></template>

            <template slot="lead">
                {{  currentQuestion.question }}
            </template>

            <hr class="my-4">
            <b-list-group>
                    <b-list-group-item @click="selectedAnswer(index)" v-for="(answer,index) in answers" :key="index" :class="[selectedIndex == index ? 'selected' : '']">{{ answer }}</b-list-group-item>
            </b-list-group>
            <b-button class="submit-ans" variant="primary" @click="submitAnswer">Submit</b-button>
            <b-button class="next-ques" variant="success" @click="next" >Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash'
export default {
    props:{
        currentQuestion:Object,
        next:Function,
        increment:Function,
    },
    data(){
        return{
            selectedIndex : null,
            correctIndex : null,
            shuffledAnswers : []
        }
    },
    computed:{
        answers(){
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        },
    },
    methods:{
        selectedAnswer(index){
            this.selectedIndex = index
        },
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswer(){
           let isCorrect = false
           if(this.selectedIndex === this.correctIndex){
               isCorrect = true
           }
           this.increment(isCorrect);
        },
    },
    watch:{
       currentQuestion:{
           immediate: true,
           handler(){
               this.selectedIndex = null,
               this.shuffleAnswers()
           }
       }
    },
}
</script>

<style>
.list-group{
    margin-bottom: 10px;
}
.submit-ans{
  margin-right: 10px;
}
.next-ques{
  margin-left: 10px;
}
.list-group-item:hover{
    background-color: #EEE;
    transition: 0.3s ease;
    cursor: pointer;
}
.selected{
    background-color: rgba(248, 156, 18, 0.856);
}
.correct{
    background-color: green;
}
.incorrect{
    background-color: red;
}
</style>
