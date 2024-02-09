<script setup>
import { ref } from 'vue';
import QuestionPanel from './components/QuestionPanel.vue';
import ScorePanel from './components/ScorePanel.vue';

const points = ref(0);
const questionsNumber = ref(0);

function updateScore(correct) {
  questionsNumber.value++;
  if (correct) points.value++;
}
</script>

<template>
  <header>
    <ScorePanel :points="points" :questions-number="questionsNumber" />
  </header>
  <main>
    <QuestionPanel @QuestionResult="updateScore" />
  </main>
</template>

<style scoped></style>
