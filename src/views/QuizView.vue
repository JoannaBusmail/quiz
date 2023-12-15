<template>
    <div>
        <QuizHeader
            :questionNumber="questionStatus"
            :progressBar="barPercentage"
        />
        <div>
            <!--EMIT -->
            <!--@nameOfTheEmitOfTheChild = a function  -->
            <Question
                v-if="!showResults"
                :question="quiz.questions[currentQuestionIndex]"
                @selectOption="onOptionSelected"
            />
            <Results
                v-else
                :quizQuestionLength="quiz.questions.length"
                :correctAnswers="numberCorrectAnswers"
            />
        </div>
        <!--No need for NEXT BUTTON, now with --  @selectOption="onOptionSelected" -- every time we click an option goes to next question-->
        <!--<button @click="currentQuestionIndex++">Next question</button>-->
    </div>
</template>

<script setup>
import { ref, watch, computed, onMounted, onUnmounted } from 'vue'
import Question from '../components/Question.vue'
import QuizHeader from '../components/QuizHeader.vue'
import Results from '../components/Result.vue'
import { useRoute } from 'vue-router'
import quizes from '../data/quizes.json'

const route = useRoute()

const quizId = parseInt(route.params.id)


const quiz = quizes.find(quiz => quiz.id === quizId)

// this set the first question as the current question
const currentQuestionIndex = ref(0)

const numberCorrectAnswers = ref(0)
const showResults = ref(false)

// ///// QUESTION STATUS /////

//--------->   const questionStatus = `${currentQuestionIndex.value + 1}/${quiz.questions.length}`
//this is not working because as is not reactive is not rerendering the index
// so we do something like this --->


// /////  REF + WATCHER //////
//so we need to use a watcher, to see if the ref index has change to update it
//The first parameter is the value to watch, the second is the function to run
// the first value is a function because we need the return value of the function

// --------> const questionStatus = ref(`${currentQuestionIndex.value + 1}/${quiz.questions.length}`)

// --------> watch(() => currentQuestionIndex.value, (newValue) =>
//             {
//                 questionStatus.value = `${newValue + 1}/${quiz.questions.length}`
//             })


// ///// COMPUTED -----BETTER SOLUTION ///////
// by doing this we dont need to use the ref and the watcher
// because the computed is reactive and is going to update the value everytime is change
// its onlye a render change, the value is not change
const questionStatus = computed(() =>
{
    return `${currentQuestionIndex.value}/${quiz.questions.length}`
})

// ///////  PROGRESS BAR ///////
const barPercentage = computed(() =>
{
    return (currentQuestionIndex.value) / quiz.questions.length * 100
})


// used in the EMIT
// when we click an option in the child component, we need to know if it is correct
// if its correct we add 1 to the number of correct answers
// and we add 1 to the current question index to go to the next question, no matter if is correct or not
//like this we do not need a next question button
const onOptionSelected = (isCorrect) =>
{
    if (isCorrect) {
        numberCorrectAnswers.value++
    }
    // currentQuestionIndex goes from 0 to 2
    // quiz.questions.length goes from 1 to 3, that's why we need to substract 1
    // so if we have the final index and the final question, we change the results to True
    //and render the Results component
    if (currentQuestionIndex.value === quiz.questions.length - 1) {
        showResults.value = true
    }

    currentQuestionIndex.value++
}


// ADD KEYBOARD SUPPORT
onMounted(() =>
{
    window.addEventListener('keydown', handleKeyPress)
})

// Remove event listener when the component is unmounted
onUnmounted(() =>
{
    window.removeEventListener('keydown', handleKeyPress)
})

// Handle key press event
const handleKeyPress = (event) =>
{
    const key = event.key.toUpperCase() // Convert to uppercase to handle both cases
    if ([ 'A', 'B', 'C', 'D' ].includes(key)) {
        // If the pressed key is one of the option labels, select the option
        const selectedOption = quiz.questions[ currentQuestionIndex.value ].options.find(option => option.label === key)
        if (selectedOption) {
            onOptionSelected(selectedOption.isCorrect)
        }
    }
}
</script>

