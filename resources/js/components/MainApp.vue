<template>
    <div>
        <HeaderComponent
            :counter="counter"
            :numCorrect="numCorrect"
            :numTotal="numTotal"
        />

        <b-container class="bv-example-row">
            <b-row>
                <b-col sm="6" offset="3">
                    <QuestionBox
                            v-if="questions.length"
                            :currentQuestion="questions[index]"
                            :next="next"
                            :previous="previous"
                            :increment="increment"
                    />                    
                </b-col>
            </b-row>
        </b-container>
    </div>
</template>

<script>
import HeaderComponent from '../components/Header.vue';
import QuestionBox from '../components/Question.vue';

export default {
    data: function(){
        return {
            questions: [],
            index: 0,
            numCorrect: 0,
            numTotal: 0,
            counter: 1
        }
    },
    components: {
        HeaderComponent,
        QuestionBox
    },
    methods: {
        next(){
            this.index++;
            this.counter++;
        },
        previous(){
            this.index--;
            this.counter--;
        },
        increment(isCorrect){
            if(isCorrect){
                this.numCorrect++;
            }
            this.numTotal++;
        }
    },
    mounted(){
        fetch('https://opentdb.com/api.php?amount=10&category=26&type=multiple',{
            method: 'get'
        })
        .then((response) => {
            return response.json();
        })
        .then((jsonData) => {
            this.questions = jsonData.results;
        })
    }
}
</script>