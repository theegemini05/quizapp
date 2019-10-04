<template>
    <div style="text-align: center">
        <b-jumbotron>
            <template v-slot:header>Quiz App</template>

            <template v-slot:lead>
                    {{currentQuestion.question}}
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item v-for="(answer, index) in answers"
                                    :key="index"
                                    @click.prevent="selectAnswer(index)"
                                    :class="answerClass(index)"
                                    >{{answer}}</b-list-group-item>
            </b-list-group>
            
            <span
                v-b-popover.hover.left.v-danger="'Renders previous question!'" 
                title="Previous"
            >
                <b-button 
                    variant="danger" href="#"
                    :disabled="counter === 0"
                    @click="counterMinus()"
                    >Previous</b-button>
            </span>
            
            <span
                v-b-popover.hover.bottom.v-primary="'Submits answer to question!'" 
                title="Submit"
            >
                <b-button 
                    variant="primary" href="#"
                    :disabled="selectedIndex === null && !answered"
                    @click="submitAnswer()"
                    >Submit</b-button>
            </span>

            <span
                v-b-popover.hover.right.v-success="'Renders next question!'" 
                title="Next"
            >
                <b-button 
                    variant="success" href="#"
                    :disabled="counter === 9"
                    @click="counterAdd()"
                    >Next</b-button>
            </span>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash';

export default {
    data: function(){
        return {
            answers: [], 
            counter: 0,
            selectedIndex: null,
            answered: false, 
            correctIndex: null
        }
    },
    props: {
        currentQuestion: Object,
        next: Function,
        previous: Function,
        increment: Function
    }, 
    watch: {
        currentQuestion: {
            immediate: true,
            handler(){
                this.shuffleAnswers();
                this.selectedIndex = null;
                this.answered = false;
            }
        }
    },
    methods: {
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
            this.answers = _.shuffle(answers);
            this.correctIndex = this.answers.indexOf(this.currentQuestion.correct_answer);
        },
        counterAdd(){
            this.counter++;
            this.next();
        },
        counterMinus(){
            this.counter--;
            this.previous();
        },
        selectAnswer(index){
            this.selectedIndex = index;
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

            return answerClass;
        },
        submitAnswer(){
            let isCorrect = false;

            if(this.selectedIndex === this.correctIndex){
                isCorrect = true;
            }

            this.answered = true;

            this.increment(isCorrect);
        }
    }
}
</script>

<style scoped>
    .btn {
        margin-top: 15px;
    }

    .lead {
        margin-top: 10px;
    }

    .list-group-item:hover {
        background: #EEE;
        cursor: pointer;
    }

    .selected{
        background-color: lightblue;
    }

    .correct{
        background-color: lightgreen; 
    }

    .incorrect{
        background-color: lightcoral;
    }
</style>