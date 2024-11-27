<template>
  <div class="cart-item-card">
    <div class="content">
      <img :src="item.image" :alt="item.topic + ' image'" class="image" />
      <div class="info">
        <h2>{{ item.topic }}</h2>
        <p>Location: {{ item.location }}</p>
        <p>Price: ${{ item.price }}</p>
        <p>Quantity: {{ item.space }}</p>
      </div>
    </div>
    <button v-on:click="props.removeCartItem(item.id)" class="remove-button">
      Remove
    </button>
  </div>
</template>

<script setup>
const props = defineProps({
  item: {
    type: Object,
    required: true,
  },
  removeCartItem: {
    type: Function,
    required: true,
  },
})
</script>

<style scoped>
.cart-item-card {
  background-color: #1e1e1e;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  text-align: center;
  padding: 20px;
  margin-bottom: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.image {
  width: 80px;
  height: 80px;
  margin-right: 20px;
}

.content {
  display: flex;
  align-items: center;
}

.info {
  text-align: left;
}

.info h2 {
  margin: 0;
  font-size: 1.5rem;
  color: #ffffff;
}

.info p {
  margin: 5px 0;
  color: #ffffff;
  font-size: 1rem;
}

.remove-button {
  background-color: #ff4d4d;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}

.remove-button:hover {
  background-color: #ff1a1a;
}
</style>
