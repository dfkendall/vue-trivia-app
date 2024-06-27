<script setup>
import { ref } from 'vue'
const emit = defineEmits(['selectedOption'])
//Change result to Boolean (default false, true if result is correct)
const props = defineProps({
  options: Array,
  color: String,
  index: Number,
  questionNumber: Number,
  result: Boolean,
  correctAnswer: String
})
const selectedAnswer = ref(null)
const handleClick = (item, i) => {
  selectedAnswer.value = i
  emit('selectedOption', item)
}
const handleAnswerStyle = (item, i) => {
  if (selectedAnswer.value !== null && i === selectedAnswer.value) {
    if (props.result) {
      return 'correct-answer'
    } else {
      return 'incorrect-answer'
    }
  } else {
    if (selectedAnswer.value !== null && item === props.correctAnswer) {
      return 'correct-answer'
    }
  }
}
</script>

<template>
  <div v-if="!questionNumber">
    <div
      class="card"
      :style="`background-color: ${item.color}`"
      v-for="item in options"
      :key="item"
      @click="$emit('selectedOption', item.value)"
    >
      {{ item.name }}
    </div>
  </div>
  <div v-else-if="index + 1 === questionNumber" class="answer-section">
    <div
      class="card"
      :style="`background-color: ${color}`"
      v-for="(item, i) in options"
      :class="handleAnswerStyle(item, i)"
      :key="item"
      @click="handleClick(item, i)"
    >
      {{ item }}
    </div>
  </div>
</template>
