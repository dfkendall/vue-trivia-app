<script setup>
//Imports
import { ref, computed } from 'vue'
import Options from './Options.vue'
import Title from './Title.vue'
import Subtitle from './Subtitle.vue'
import { categories, difficulties, test } from '../assets/data'

//Refs
const category = ref(null)
const difficulty = ref(null)
const questions = ref([])
const questionNumber = ref(1)
const correctGuesses = ref(0)
const questionResult = ref(false)

//Functions
const handleGuess = (selection, correct) => {
  if (selection === correct) {
    correctGuesses.value = correctGuesses.value + 1
    questionResult.value = true
  }
  setTimeout(() => {
    questionNumber.value = questionNumber.value + 1
    questionResult.value = false
  }, 1500)
}
const getQuestions = async (cat) => {
  category.value = cat
  // let url = 'https://opentdb.com/api.php?amount=50&category=12&difficulty=easy&type=multiple'
  // // let url = `https://opentdb.com/api.php?amount=20&category=${category.value}&difficulty=${difficulty.value}&type=multiple`
  // const data = await fetch(url).then((response) => {
  //   if (!response.ok) {
  //     throw new Error('There was an error')
  //   }
  //   return response.json()
  // })
  // data.results.forEach((obj) => {
  //   const partialObj = {}
  //   partialObj.question = obj.question
  //   partialObj.correct_answer = obj.correct_answer
  //   partialObj.answers = obj.incorrect_answers
  //   partialObj.answers.push(partialObj.correct_answer)
  //   questions.value.push(partialObj)
  // })
  test.forEach((obj) => {
    const partialObj = {}
    partialObj.question = obj.question
    partialObj.correct_answer = obj.correct_answer
    partialObj.answers = obj.incorrect_answers
    partialObj.answers.push(partialObj.correct_answer)
    partialObj.answers = shuffle(partialObj.answers)
    questions.value.push(partialObj)
  })
}
const shuffle = (array) => {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1))
    ;[array[i], array[j]] = [array[j], array[i]]
  }
  return array
}

//Computed properties
const numCorrectCounter = computed(
  () => `Correct guesses: ${correctGuesses.value}/${questionNumber.value - 1}`
)
</script>

<template>
  <div v-if="!difficulty">
    <Title text="Just Another Trivia App!" fontSize="50" />
    <div class="justify-center">
      <Subtitle text="Please select a difficulty:" />
    </div>
    <div class="justify-center">
      <Options
        @selected-option="(value) => (difficulty = value)"
        :options="difficulties"
        class="difficulty-card"
      />
    </div>
  </div>
  <div v-else-if="difficulty && !category">
    <Subtitle text="Please select a category:" />
    <div class="categories">
      <Options @selected-option="(value) => getQuestions(value)" :options="categories" />
    </div>
  </div>
  <div v-for="(question, index) in questions" :key="question">
    <p v-if="questionNumber === index + 1">{{ question.question }}</p>
    <Options
      @selected-option="(value) => handleGuess(value, question.correct_answer)"
      :options="question.answers"
      color="#888888"
      :index="index"
      :questionNumber="questionNumber"
      :result="questionResult"
      :correctAnswer="question.correct_answer"
    />
  </div>
  <h3 v-if="questionNumber > 1">{{ numCorrectCounter }}</h3>
</template>
