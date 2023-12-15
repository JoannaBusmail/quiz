<template>
    <div class="results">
        <p>Your Results...</p>
        <h1>{{ finalResult }}</h1>
        <RouterLink
            to="/"
            class="play-again"
        >Play Again</RouterLink>
    </div>
</template>

<script setup>
import { computed } from 'vue'
import { RouterLink } from 'vue-router'

const props = defineProps({
    quizQuestionLength: Number,
    correctAnswers: Number
})

const finalResult = computed(() =>
{
    return `${props.correctAnswers} / ${props.quizQuestionLength}`
})
</script>



<style scoped>
.results {
    text-align: center;
    padding: 100px 0;
}

p {
    font-size: 25px;
}

h1 {
    font-size: 80px;
}

.play-again {
    display: inline-block;
    margin-top: 50px;
    padding: 10px 20px;
    border: 2px solid #cbc8c8;
    border-radius: 5px;
    text-decoration: none;
    color: #000;
}
</style>