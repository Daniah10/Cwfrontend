<script setup>
import{ref} from "vue"
import LessonCard from "./components/LessonCard.vue";
const lessons= ref([])
const getLessons= async () => {
  const response= await fetch ("https://applicationbackend-58pp.onrender.com/lessons")
  const data= await response.json()
  lessons.value= data
}
getLessons()

</script>

<template>
<LessonCard v-for="lesson in lessons" :key="lesson.id" :lesson="lesson"/>
  

  
</template>

<style scoped>

</style>
