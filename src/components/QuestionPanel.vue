<template>
  <div>
    <h1>Pregunta</h1>
    <button @click="getQuestion">Nueva pregunta</button>
    <h2>{{ question }}</h2>
    <hr>
    <button :disabled="isResponse"
      :class="{ wrong: isResponse && answer == userResponse, correct: isResponse && answer == correct }"
      v-for="answer in  answers  " @click="checkResponse(answer)">
      {{ answer }}
    </button>
    <hr>
    <ResultImage v-if="isResponse" :result="yesOrNo" />
  </div>
</template>
<script setup>
import { ref, computed } from 'vue';
import ResultImage from './ResultImage.vue';
const emit = defineEmits(['QuestionResult']);

const question = ref('');
const incorrects = ref([]);
const correct = ref('');
const isResponse = ref(false);
const userResponse = ref('');
const yesOrNo = ref('');

const answers = computed(() => {
  const answers = [];
  answers.push(...incorrects.value)
  answers.push(correct.value);
  answers.sort(() => Math.random() - 0.5);
  return answers;
});

async function getQuestion() {
  try {
    const response = await fetch('https://opentdb.com/api.php?amount=1');
    if (!response.ok) {
      throw ('Fallo!!');
    }
    const json = await response.json();
    if (json.response_code !== 0) {
      throw ('Fallo!!');
    }
    const quiz = json.results[0];
    question.value = quiz.question;
    incorrects.value = quiz.incorrect_answers;
    correct.value = quiz.correct_answer;
    isResponse.value = false;
    userResponse.value = '';
    yesOrNo.value = '';
    console.log(json);
  } catch (e) {
    console.log('Hay un error en la petición de preguntas. ' + e);
    setTimeout(getQuestion, (Math.random() * 3 + 2) * 1000);
  }
}

function checkResponse(a) {
  console.log(a);
  isResponse.value = true;
  userResponse.value = a;
  if (a == correct.value) {
    console.log('Correcta');
    emit('QuestionResult', true);
    yesOrNo.value = 'yes';
  } else {
    console.log('Incorrecta');
    emit('QuestionResult', false);
    yesOrNo.value = 'no';
  }
  setTimeout(() => getQuestion(), 2000);
}
</script>
<style scoped>
h1 {
  color: darkgreen;
}

.wrong {
  background-color: red;
}

.correct {
  background-color: green;
}
</style>