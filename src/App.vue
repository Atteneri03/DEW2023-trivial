<script setup>
import { ref } from 'vue';
import QuestionPanel from './components/QuestionPanel.vue';
import ScorePanel from './components/ScorePanel.vue';

const points = ref(0);
const questionNumber = ref(0);

function updateScore(correct){
  questionNumber.value++;
  if(correct) points.value++;
}
</script>

<template>
  <header>
    <ScorePanel :points="points" :questionNumber="questionNumber"/>
  </header>
  <main>
    <QuestionPanel @QuestionResult="updateScore"/>
  
  </main>
</template>

<style scoped>

</style>
