<script setup>
import { ref, computed } from 'vue'

const questions = ref([
  {
    question: 'What are the first 10 digits of PI',
    answer: 0,
    options: [
      '3.1415926535',
      '3.1417926535',
      '3.1415926545'
    ],
    selected: null
  },

  {
    question: 'What does this character mean 苹果',
    answer: 2,
    options: [
      'Chair',
      'ur mom',
      'Apple'
    ],
    selected: null
  },

  {
    question: 'what is the first video game ever made',
    answer: 1,
    options: [
      'Mario',
      'Pong',
      'Tetris',
      'Tic Tac Toe'
    ],
    selected: null
  }
])

const quizzCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
  let value = 0
  questions.value.map(q => {
    if (q.selected != null && q.selected == q.answer) {
      value++
    }
  })
  return value
})

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value]
  question.index = currentQuestion.value
  return question
})

const SetAnswer = evt => {
  questions.value[currentQuestion.value].selected = evt.target.value
  evt.target.value = null
}

const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++
  }
  else {
    quizzCompleted.value = true
  }
}
</script>

<template>
  <main class="app">
    <h1>Quiz</h1>

    <section class="quiz" v-if="!quizzCompleted">

      <div class="quiz-info">
        <span class="question">
          {{ getCurrentQuestion.question }}
        </span>
        <span class="score">
          {{ score }} / {{ questions.length }}
        </span>
      </div>

      <div class="options">
        <label v-for="(option, index) in getCurrentQuestion.options" :key="index" :class="`option ${getCurrentQuestion.selected == index
            ? index == getCurrentQuestion.answer
              ? 'correct'
              : 'wrong'
            : ''
          } ${getCurrentQuestion.selected != null &&
            index != getCurrentQuestion.selected
            ? 'disabled'
            : ''
          }`">
          <input 
          type="radio" 
          :name="getCurrentQuestion.index" 
          :value="index" 
          v-model="getCurrentQuestion.selected"
          :disabled="getCurrentQuestion.selected" 
          @change="SetAnswer" />

          <span>{{ option }}</span>
        </label>
      </div>

      <button
      @click ="NextQuestion"
      :disabled="!getCurrentQuestion.selected">
      {{
        getCurrentQuestion.index == questions.length - 1
          ?'Finish'
          : getCurrentQuestion.selected == null
            ?'Select an option'
            :'Next Question'
      }}

      </button>

    </section>

    <section v-else>
      <h2>You have finished the Quiz</h2>
      <p>Your score is {{ score }} / {{ questions.length }}</p>
    </section>

  </main>
</template>

