<template>
    <div class="question-box-container">
        <b-jumbotron lead="Bootstrap v4 Components for Vue.js 2">
            <template #lead>
                {{currentQuestion.question}}
            </template>

    <hr class="my-4">


    <b-list-group v-for="(answer, index) in shuffledAnswers" :key="index">
        <b-list-group-item 
            @click.prevent="selectAnswer(index)" 
            :class="answerClass(index)"
        >
        {{answer}}
        </b-list-group-item>
    </b-list-group>

    <b-button variant="primary" @click="submitAnswer" :disabled="selectedIndex === null || answered">Submit</b-button>
        <b-button @click="next" variant="success" :disabled="selectedIndex === null">Next</b-button>
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
             correctIndex: null,
             answered: false,
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
        //when the currentQuestion changes, we want to shuffle the answers and set the correctIndex
        currentQuestion: {
            immediate: true,
            handler(){
                this.selectedIndex = null
                this.shuffleAnswers()
                this.answered = false
                console.log(this.correctIndex, this.currentQuestion.correct_answer) 
            },
        },  
    },
    methods: {
        //
        selectAnswer(index){
            this.selectedIndex = index
        },
        //shuffle answers array using lodash
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswer(){
            let isCorrect = false
            if (this.selectedIndex == this.correctIndex){
                isCorrect = true
            }
            this.answered = true
            this.increment(isCorrect)
        },
        answerClass(index){
            let answerClass = ''

            if(!this.answered && this.selectedIndex === index){
                answerClass = 'selected'
            }
            else if(this.answered && this.correctIndex === index){
                answerClass = 'correct'
            }
            else if(this.answered && this.selectedIndex === index && this.correctIndex !== index){
                answerClass = 'incorrect'
            }
            return answerClass
        },
        mounted(){
            this.shuffleAnswers()
            
        },
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
        background-color: rgb(212, 73, 73);
    }
</style>