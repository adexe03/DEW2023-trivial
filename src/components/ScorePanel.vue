<template>
  <h1>
    Nº Preguntas: {{ questionsNumber }} | Puntos: {{ points }}
  </h1>
</template>
<script setup>
defineProps(['questionsNumber', 'points']);
</script>
<style></style>