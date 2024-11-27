<script setup>
import{ref} from "vue"
import LessonCard from "./components/LessonCard.vue";
import SortSearchBar from "./components/SortSearchBar.vue";
import SiteHeader from "./components/SiteHeader.vue";
import CartDrawar from "./components/CartDrawar.vue";
const lessons= ref([])
const cart= ref([])
const sortProperty= ref("")
const sortOrder= ref("")
const showDrawer= ref(false)
const getLessons= async () => {
  const response= await fetch ("https://applicationbackend-58pp.onrender.com/lessons")
  const data= await response.json()
  lessons.value= data
}
getLessons()
const toggleCart= ()=> {
  showDrawer.value= ! showDrawer.value
} 
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

const buy=(id)=> {
  const lesson = lessons.value.find((lesson) => lesson.id === id)
  lesson.space -- 
  lessons.value = [...lessons.value]
  changeSort(sortProperty.value, sortOrder.value)

const existingItem= cart.value.find(item => item.id === id)
if(! existingItem){ 
  cart.value.push({
  id: lesson.id, 
  topic: lesson.topic, 
  location: lesson.location, 
  price: lesson.price, 
  image: lesson.image, 
  space: 1,
})
}
else{existingItem.space ++}
}
const removeCartItem=(id)=>{
  const lesson = lessons.value.find(lesson => lesson.id === id)
  const cartItem = cart.value.find(item => item.id === id)

  lesson.space += cartItem.space

  cart.value = cart.value.filter(item => item.id !== id)

  changeSort(sortProperty.value, sortOrder.value)
  if(cart.value.length === 0)
  toggleCart()
}
const checkout= async(name, phone)=> {
  const response = await fetch('https://applicationbackend-58pp.onrender.com/order', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      name: name,
      phone: phone,
      items: cart.value.map(item => ({
        id: item.id,
        space: item.space,
      })),
    }),
  })

  if (!response.ok) {
    showSnackbar('There was an error placing your order. Please try again later', false)
    return
  }

  showSnackbar('Order placed updating lesson availability...', true)

  for (let item of cart.value) {
    const lesson = lessons.value.find(lesson => lesson.id === item.id)
    const response2 = await fetch('https://applicationbackend-58pp.onrender.com/lesson', {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        id: item.id,
        space: lesson.space,
      }),
    })
    if (!response2.ok) {
      showSnackbar(`There was an error updating the lesson ${item.id} availability. Please try again later`, false)
    }
  }
  cart.value =[]
  toggleCart()
  showSnackbar("Your Order has been Placed Successfully!", true)
}
const showSnackbar = (message, success) => {
  const snackbar = document.createElement('div')
  snackbar.className = 'snackbar'
  snackbar.textContent = message
  document.body.appendChild(snackbar)

  if(success) {
    snackbar.classList.add('success')
  }
  else {
    snackbar.classList.add('error')
  }

  setTimeout(() => {
    snackbar.classList.add('show')
  }, 100)

  setTimeout(() => {
    snackbar.classList.remove('show')
    setTimeout(() => {
      document.body.removeChild(snackbar)
    }, 300)
  }, 3000)
}
</script>

<template>
<SiteHeader siteName="subject store" @toggleCart="toggleCart" :cartDisabled="cart.length === 0"/>
<SortSearchBar :changeSort="changeSort"/>  
<LessonCard v-for="lesson in lessons" :key="lesson.id" :lesson="lesson" :addToCart="buy"/>
<CartDrawar :isDrawerVisible="showDrawer" :cartItems="cart" :removeCartItem="removeCartItem" :checkout="checkout"/>
  

  
</template>

<style scoped>

</style>
