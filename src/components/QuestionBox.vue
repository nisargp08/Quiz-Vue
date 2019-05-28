<template>
    <div>
        <b-jumbotron>
            <template slot="header"></template>

            <template slot="lead">
                {{  currentQuestion.question | decoder }}
            </template>

            <hr class="my-4">
            <b-list-group>
                    <b-list-group-item @click="selectedAnswer(index)" v-for="(answer,index) in shuffledAnswers" :key="index" :class="classSelector(index)">
                    {{ answer | decoder }}
            </b-list-group-item>
            </b-list-group>
            <b-button class="submit-ans" variant="primary" @click="submitAnswer" :disabled="selectedIndex === null || answered">Submit</b-button>
            <b-button class="next-ques" title="Continue to Next Question" variant="success" @click="next" :disabled="!answered">Next</b-button>
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
            shuffledAnswers : [],
            answered : false,
        }
    },
    computed:{
        answers(){
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        },
    },
    filters: {
        decoder(str) {
        var textArea = document.createElement("textarea");
        textArea.innerHTML = str;
        return textArea.value;
        }
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
           this.answered = true;
        },
        classSelector(index){
            let answerClass = ''
            if(!this.answered && this.selectedIndex === index){
                answerClass = 'selected'
            }
            else if(this.answered && this.correctIndex === index){
                answerClass = 'correct'
            }
            else if(this.answered && this.selectedIndex === index && this.selectedIndex !== this.correctIndex){
                answerClass = 'incorrect'
            }
            return answerClass;
        },
    },
    watch:{
       currentQuestion:{
           immediate: true,
           handler(){
               this.selectedIndex = null,
               this.shuffleAnswers()
               this.answered = false
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
    background-color: lightgreen;
}
.incorrect{
    background-color: rgb(250, 59, 59);
}
</style>
