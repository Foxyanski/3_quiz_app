<template>
  <div class="container">
    <QuizHeader :questionStatus="questionStatus" :barPercentage="barPercentage" />
    <div>
      <Question
        v-if="!showResults"
        :question="quiz.questions[currentQuestionIndex]"
        @selectOption="onOptionSelected"
      />
      <QuizResults
        v-else
        :quizQuestionLength="quiz.questions.length"
        :numberOfCorrectAnswers="numberOfCorrectAnswers"
      />
    </div>
  </div>
</template>

<script setup>
import Question from '../components/Question.vue'
import QuizHeader from '../components/QuizHeader.vue'
import { useRoute } from 'vue-router'
import { ref, computed } from 'vue'
import quizes from '../data/data.json'
import QuizResults from '../components/QuizResults.vue'

const route = useRoute()
const quizId = parseInt(route.params.id)
const quiz = quizes.find((q) => q.id === quizId)
const currentQuestionIndex = ref(0)
const numberOfCorrectAnswers = ref(0)
const showResults = ref(false)

const onOptionSelected = (isCorrect) => {
  if (isCorrect) {
    numberOfCorrectAnswers.value++
  }
  if (quiz.questions.length - 1 === currentQuestionIndex.value) {
    showResults.value = true
  }

  currentQuestionIndex.value++
}

// const questionStatus = ref(`${currentQuestionIndex.value}/${quiz.questions.length}`)
// watch(
//   //if currentQI is changed
//   () => currentQuestionIndex.value,
//   () => {
//     //then re-render questionStatus (it will not re-render automatically because its two different vars)
//     questionStatus.value = `${currentQuestionIndex.value}/${quiz.questions.length}`
//   }
// )

const questionStatus = computed(() => `${currentQuestionIndex.value}/${quiz.questions.length}`)
const barPercentage = computed(
  () => `${(currentQuestionIndex.value / quiz.questions.length) * 100}%`
)
</script>

<style lang="scss" scoped>
.container {
  margin-left: 2rem;
}
</style>
