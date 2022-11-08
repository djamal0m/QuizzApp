<template>
    <div class="question-box-container">
        <b-jumbotron lead="Bootstrap v4 Components for Vue.js 2">
            <template #lead>
                {{currentQuestion.question}}
            </template>

    <hr class="my-4">


    <b-list-group v-for="(answer, index) in answers" :key="index">
        <b-list-group-item @click="selectAnswer(index)" :class="[selectedIndex === index ? 'selected' : '']">{{answer}}</b-list-group-item>
    </b-list-group>

    <b-button variant="primary" @click="submitAnswer">Submit</b-button>
        <b-button @click="next" variant="success" >Next</b-button>
    </b-jumbotron>
    </div>
</template> 

<script>  
import _ from 'lodash'

export default {
    props: { 
        currentQuestion: Object,
        next: Function,
        increment: Function
    },
    data(){
        return {
             selectedIndex: null,
             shuffledAnswers: [],
             correctIndex: null
        }
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    },
    watch: {
        currentQuestion(){ 
            this.selectedIndex = null
            this.shuffleAnswers()
        },
    },
    methods: {
        selectAnswer(index){
            this.selectedIndex = index
        },
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
        },
        submitAnswer(){
            let isCorrect = false
            if (this.selectedIndex == this.correctIndex){
                isCorrect = true
            }
            this.increment(isCorrect)
        }
    },
    mounted(){
        this.shuffleAnswers()
    }
}
</script>

<style scoped>
    .list-group{
        margin-bottom: 15px;
    }

    .list-group-item:hover{
        background: #eee;
        cursor: pointer;
    }
    .btn{
        margin: 0 90px; 
    }
    .selected{
        background-color: rgb(162, 204, 227);
    }
    .correct{
        background-color: rgb(22, 169, 25);
    }
    .incorrect{
        background-color: red;
    }
</style>