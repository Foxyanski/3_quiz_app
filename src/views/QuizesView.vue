<template>
  <div class="container">
    <header>
      <h1>Quizes</h1>
      <input v-model.trim="search" type="text" placeholder="Search...." />
    </header>
    <div class="options-container">
      <Card v-for="quiz in quizes" :key="quiz.id" :quiz="quiz" />
    </div>
  </div>
</template>

<script setup>
import q from '../data/data.json'
import { ref, watch } from 'vue'

import Card from '../components/Card.vue'

const quizes = ref(q)
const search = ref('')

watch(search, () => {
  quizes.value = q.filter((quiz) => quiz.name.toLowerCase().includes(search.value.toLowerCase()))
})
</script>

<style scoped>
.container {
  margin-left: 2rem;
}
header {
  margin-bottom: 10px;
  margin-top: 30px;
  display: flex;
  align-items: center;
}

header h1 {
  font-weight: bold;
  margin-right: 30px;
  color: black;
}

header input {
  border: none;
  background-color: rgb(207, 200, 200);
  padding: 10px;
  border-radius: 5px;
}

.options-container {
  display: flex;
  flex-wrap: wrap;
  margin-top: 40px;
}

/* Card */
</style>
