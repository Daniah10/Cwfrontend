<template>
  <header class="site-header">
    <div class="site-brand">
      <p class="site-name">{{ siteName }}</p>
    </div>
    <button
      class="cart-button"
      v-on:click="$emit('toggleCart')"
      :disabled="props.cartDisabled"
    >
      <span class="cart-text">Cart</span>
      <img
        src="https://img.icons8.com/ios-filled/50/000000/shopping-cart.png"
        alt="Cart"
        class="cart-icon"
      />
    </button>
  </header>
</template>

<script setup>
const props = defineProps({
  siteName: {
    type: String,
    default: 'My Website',
  },
  cartDisabled: {
    type: Boolean,
    default: true,
  },
})
</script>

<style scoped>
.site-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 20px;
  background: #333;
  color: #fff;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  margin: 0 10rem 0 10rem;
  border-radius: 0 0 20px 20px;
}

.site-brand {
  text-decoration: none;
}

.site-name {
  color: #fff;
  font-size: 2rem;
  margin: 0;
  text-decoration: none;
}

.cart-button {
  background: #f4f4f7;
  border: none;
  border-radius: 20px;
  cursor: pointer;
  padding: 8px 12px;
  display: flex;
  align-items: center;
  opacity: 1;
}

.cart-button:hover {
  background: #c1c1c2;
}

.cart-button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.cart-text {
  margin-right: 5px;
  font-weight: bold;
}

.cart-icon {
  width: 24px;
  height: 24px;
}
</style>
