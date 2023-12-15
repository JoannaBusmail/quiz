<template>
  <header>
    <h1>Quizes</h1>
    <input
      v-model.trim="search"
      type="text"
      placeholder="Search..."
    >
  </header>
  <div class="options-container">
    <!--APPEAR-->
    <!--this means the animation will happen when the component is rendered-->
    <!--@before-enter listen to an event fromt the animation, before entering the dom-->
    <!--@enter listen to an event fromt the animation, when entering the dom-->
    <!--@after-enter listen to an event fromt the animation, after entering the dom and the animation is finished-->
    <TransitionGroup
      appear
      @before-enter="beforeEnter"
      @enter="enter"
      @after-enter="afterEnter"
    >
      <!--we need to get the index for the animation of each card individually-->
      <!--and need to add :data-whateverName="index"-->
      <Card
        v-for="(quiz, index) in quizes"
        :key="quiz.id"
        :quiz="quiz"
        :data-index="index"
      ></Card>
    </TransitionGroup>
  </div>
  <!--BEFORE DOING CARD COMPONENT 
        <div
          v-for="quiz in quizes"
          :key="quiz.id"
          class="card"
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
  -->
</template>
  
<script setup>
// q is the original array of quizes, in the constant for the quizes array
import q from '../data/quizes.json'
import { ref, watch } from 'vue'
import Card from '../components/Card.vue'
import gsap from 'gsap'

// need to save the json in a const so it can be iterated over
const quizes = ref(q)
const search = ref('')

// WATCH
// 2 Paramenters: 1st is the value to watch, 2nd is the function to run
// we need this because qhn search changes, quizes should change too
watch(search, () =>
{
  quizes.value = q.filter(quiz =>
  {
    return quiz.name.toLowerCase().includes(search.value.toLowerCase())
  })
})

// LIFE CYCLE HOOKS FOR ANIMATIONS METHODS 
//by the logs we can see the order of the methods appearing
// specify the styles in here for animating cards individually
const beforeEnter = (el) =>
{
  //card-enter-from
  console.log('beforeEnter')
  el.style.opacity = 0
  el.style.transform = 'translateY(-100px)'
}

const enter = (el) =>
{
  //card-enter-to
  console.log('enter')
  //----  el.style.opacity = 1
  //----  el.style.transform = 'translateY(0)'
  //at this point the animation is not happening
  //because we do nos define the card-enter-active where we define time, type of animation, etc
  //we need a library---- npm install gsap
  // we have to do this -->
  // with the delay we can delay the apperance of each card by it index
  // add the index in the v-for of th card component
  // then we accedd that index in the delay
  gsap.to(el, { y: 0, opacity: 1, duration: 0.3, delay: el.dataset.index * 0.3 })

}

const afterEnter = () =>
{
  console.log('afterEnter')
}

</script>
  ```
<style scoped>
header {
  margin-bottom: 50px;
  margin-top: 30px;
  display: flex;
  align-items: center;
}

h1 {
  font-weight: bold;
  margin-right: 30px;
}

header input {
  border: none;
  background-color: rgba(128, 128, 128, 0.1);
  padding: 10px;
  border-radius: 5px;
}

.options-container {
  display: flex;
  flex-wrap: wrap;
}

/*CARD TRANSITION*/
/* THIS IS FOR ALL THE CARDS --- TRANSITIONGROUP HAD NAME="CARD"*/
/* NOW WE WANT ANIMATION FOR EACH ONE*/
/*SO STYLES SHOULD BE DONE WITH JS/*
/*.card-enter-from {
  opacity: 0;
  transform: translateY(-50px);
}

.card-enter-to {
  opacity: 1;
  transform: translateY(1);
}

.card-enter-active {
  transition: all 0.5s ease;
}*/
</style>