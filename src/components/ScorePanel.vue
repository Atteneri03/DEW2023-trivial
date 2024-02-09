<template>
  <h1>
    Nº de preguntas: {{ questionNumber }} | Puntos: {{ points }}
  </h1>
</template>


<script setup>
defineProps(['questionNumber' , 'points']);
</script>


<style scoped>
  h1{
    padding: 8px;
    background-color: lightseagreen;
  }
</style>