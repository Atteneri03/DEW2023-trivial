<template>
  <div>
    <h1>Pregunta 
      <button class="new" @click="getQuestion">Nueva Pregunta</button>
    </h1>
    <h2>{{ question }}</h2>
    <hr>
    <button v-for="answer in answers" 
    :disabled="isResponse" 
    :class="{ wrong : isResponse && answer == userResponse ,
    correct : isResponse && answer == correct}" @click="checkResponse(answer)">{{ answer }}</button>
  <hr>
  <ResultImage v-if = "isResponse && userResponse.length" result="yesOrNo"/>
  
  </div>
</template>


<script setup>
import { ref , computed} from 'vue';
import ResultImage from './ResultImage.vue';

const emit = defineEmits(['QuestionResult']);

const question = ref('');
const incorrects = ref([]);
const correct = ref('');
const isResponse = ref(true);
const userResponse = ref('');
const yesOrNo = ref('');

const answers = computed(()=>{
  const answers = [];
  answers.push(...incorrects.value);
  answers.push(correct.value);
  answers.sort(()=>Math.random()*-0,5);
  return answers;
})

async function getQuestion(){
  try {
    const response = await fetch('https://opentdb.com/api.php?amount=1');
    if(!response.ok){
      throw('Fallo');
    }
    const json = await response.json();

    if(json.response_code!=0){
      throw('Fallo');
    }

    const quiz = json.results[0];
    question.value = quiz.question;
    incorrects.value = quiz.incorrect_answers;
    correct.value = quiz.correct_answer;
    isResponse.value = false;
    userResponse.value = '';
    yesOrNo.value = "";


    console.log(json);
  } catch (e){
    console.log("Error al buscar la pregunta: " + e);
    setTimeout(getQuestion,(Math.random()*4+4)*1000);
  }
  
}

function checkResponse(answer){
  isResponse.value = true;
  userResponse.value = answer;
  if(answer == correct.value){
    console.log("Correcto");
    emit('QuestionResult' , true);
    yesOrNo.value = "yes";
  } else{
    console.log("Incorrecto");
    emit('QuestionResult' , false);
    yesOrNo.value = "no";


  }

  setTimeout(() => getQuestion(), 2000);
}


</script>


<style scoped>
  h1{
    padding: 8px;
    background-color: palevioletred;
  }

  button{
    display: inline-block;
    padding: 10px;
    margin-left: 30px;
    font-size: 15px;
    cursor: pointer;
    text-align: center;
    color: #000000;
    border: none;
    background-color: lightblue;
    border-radius: 15px;

  }

  .new{
    background-color: #ffb3b3;
  }

  .wrong {
    background-color: rgb(255, 90, 90);

  }

  .correct{
    background-color: lightgreen;

  }
</style>