<template>
  <div class="drawer" :class="{ open: props.isDrawerVisible }">
    <div class="drawer-header">
      <h2>Your Cart</h2>
    </div>
    <ul class="drawer-list">
      <li v-for="(cartItem, index) in props.cartItems" :key="index" class="drawer-item">
        <CartItemCard :item="cartItem" :removeCartItem="props.removeCartItem" />
      </li>
    </ul>
    <div class="checkout-section">
      <div class="checkout-header">
        <h3>Checkout</h3>
        <h3>Total Price: ${{ totalPrice }}</h3>
      </div>
      <label for="customer-name">Name:</label>
      <input type="text" id="customer-name" v-model="customerName" placeholder="Enter your name" />
      <p v-if="!isCustomerNameValid" class="error-message">Name can only contain letters.</p>
      <label for="customer-phone">Phone:</label>
      <input type="tel" id="customer-phone" v-model="customerPhone" placeholder="Enter your phone" />
      <p v-if="!isCustomerPhoneValid" class="error-message">Phone must contain digits only.</p>
      <button v-on:click="props.checkout(customerName, customerPhone)" class="checkout-btn" :disabled="!isFormValid">Checkout</button>
    </div>
  </div>
</template>

<script setup>
import {ref,watch} from 'vue'
import CartItemCard from './CartItemCard.vue'

const props = defineProps({
  cartItems: {
    type: Array,
    required: true,
  },
  removeCartItem: {
    type: Function,
    required: true,
  },
  checkout: {
    type: Function,
    required: true,
  },
  isDrawerVisible: {
    type: Boolean,
    required: true,
  },
})
const customerName = ref('')
const customerPhone = ref('')
const isCustomerNameValid = ref(true)
const isCustomerPhoneValid = ref(true)
const isFormValid = ref(false)
const totalPrice = ref(0)

watch([customerName, customerPhone], () => {
  validateCustomerName()
  validateCustomerPhone()
  isFormValid.value = isCustomerNameValid.value && isCustomerPhoneValid.value && customerName.value && customerPhone.value
})

const validateCustomerName = () => {
  const nameRegex = /^[a-zA-Z]+$/
  isCustomerNameValid.value = nameRegex.test(customerName.value)
}

const validateCustomerPhone = () => {
  const phoneRegex = /^\d+$/
  isCustomerPhoneValid.value = phoneRegex.test(customerPhone.value)
}

watch(props.cartItems, () => {
  totalPrice.value = props.cartItems.reduce((acc, item) => acc + item.price * item.space, 0)
})

</script>

<style scoped>
.drawer {
  position: fixed;
  top: 0;
  right: -100%;
  width: 500px;
  height: 100%;
  background-color: #333;
  box-shadow: -2px 0 5px rgba(0, 0, 0, 0.5);
  transition: right 0.3s ease;
  z-index: 500;
  padding: 80px 20px 20px 20px;
  display: flex;
  flex-direction: column;
  color: #ffffff;
  transition: right 0.7s ease;
}

.drawer.open {
  right: 0;
}

.drawer-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.drawer-list {
  list-style: none;
  padding: 0;
  margin: 0;
  flex-grow: 1;
  overflow-y: auto;
}

.drawer-item {
  margin-bottom: 10px;
}

.checkout-section {
  margin-top: 20px;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

input[type='text'],
input[type='tel'] {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.error-message {
  color: red;
  font-size: 0.9rem;
}

.checkout-btn {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 10px;
  border-radius: 4px;
  cursor: pointer;
  width: 100%;
}

.checkout-btn:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.checkout-btn:hover:not(:disabled) {
  background-color: #45a049;
}

.checkout-header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
}
</style>