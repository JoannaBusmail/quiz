<template>
    <div
        class="card"
        @click="navigateToQuiz"
    >
        <img
            :src="quiz.img"
            alt=""
        >
        <div class="card-text">
            <h2>{{ quiz.name }}</h2>
            <p>{{ quiz.questions.length }} questions</p>
        </div>
    </div>
</template>



<script setup>
import { useRouter } from 'vue-router'


const props = defineProps({
    quiz: Object
})

const router = useRouter()


//instead of using router link we use programatic appraoch
// this means we use the router object to navigate to the quiz
const navigateToQuiz = () =>
{
    router.push(`/quiz/${props.quiz.id}`)
}



</script>



<style scoped>
.card {
    width: 300px;
    overflow: hidden;
    border-radius: 2%;
    box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.1);
    margin-bottom: 35px;
    margin-right: 20px;
    cursor: pointer;
}

.card img {
    width: 100%;
    height: 190px;
    object-fit: cover;
    margin: 0;
}

.card .card-text {
    padding: 0 5px;
}

.card .card-text h2 {
    margin-top: 5px;
    font-weight: bold;

}
</style>