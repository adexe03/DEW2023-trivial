<template>
  <h1>{{ result }}</h1>
  <img :src="imageURL" :alt="result">
</template>
<script setup>
import { ref, onMounted } from 'vue';
const props = defineProps(['result']);

const imageURL = ref('');

async function getImage() {
  try {
    const response = await fetch('https://yesno.wtf/api?force=' + props.result)
    const json = await response.json();
    imageURL.value = json.image;
  } catch (error) {
    console.log('Error al cargar la imagen. ' + error);
  }
}

onMounted(() => {
  getImage();
});
</script>
<style scoped></style>