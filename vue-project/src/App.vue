<script setup>
import{ref} from "vue"
import LessonCard from "./components/LessonCard.vue";
import SortSearchBar from "./components/SortSearchBar.vue";
const lessons= ref([])
const sortProperty= ref("")
const sortOrder= ref("")
const getLessons= async () => {
  const response= await fetch ("https://applicationbackend-58pp.onrender.com/lessons")
  const data= await response.json()
  lessons.value= data
}
getLessons()

const changeSort= (property, order)=> {
  if(! property || ! order) {
    return
  } 
  sortProperty.value = property 
  sortOrder.value = order
  
  lessons.value.sort((a, b) => {
    if (property === 'space' || property === 'price') {
      return order === 'asc' ? Number(a[property]) - Number(b[property]) : Number(b[property]) - Number(a[property])
    } else {
      return order === 'asc' ? a[property].localeCompare(b[property]) : b[property].localeCompare(a[property])
    }
  })

  lessons.value = [...lessons.value]
}

</script>

<template>
<SortSearchBar :changeSort="changeSort"/>  
<LessonCard v-for="lesson in lessons" :key="lesson.id" :lesson="lesson"/>
  

  
</template>

<style scoped>

</style>
