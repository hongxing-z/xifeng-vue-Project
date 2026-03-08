<template>
  <div class="cart-container">
    <div class="cart-header">
      <h1>购物车</h1>
      <p>共 {{ cartItems.length }} 件商品</p>
    </div>

    <div class="cart-items">
      <div v-for="(item, index) in cartItems" :key="index" class="cart-item">
        <div class="item-checkbox">
          <input type="checkbox" v-model="item.selected" />
        </div>

        <div class="item-image">
          <img :src="item.image" alt="商品图片" />
        </div>

        <div class="item-info">
          <h3>{{ item.name }}</h3>
          <p class="item-desc">{{ item.description }}</p>
          <p class="item-price">¥{{ item.price }}</p>
        </div>

        <div class="item-actions">
          <div class="quantity-control">
            <button @click="decreaseQuantity(index)">-</button>
            <span>{{ item.quantity }}</span>
            <button @click="increaseQuantity(index)">+</button>
          </div>

          <div class="item-buttons">
            <button @click="removeItem(index)">删除</button>
          </div>
        </div>
      </div>
    </div>

    <div class="checkout-area">
      <div class="total-info">
        <p>合计：¥{{ totalAmount }}</p>
      </div>

      <button class="checkout-btn" @click="checkout">结算</button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const cartItems = ref([
  {
    id: 1,
    name: '混色火漆印章蜡粒',
    description: '信封封口用，多种颜色可选',
    image: 'https://via.placeholder.com/100',
    price: 7.12,
    quantity: 1,
    selected: true
  },
  {
    id: 2,
    name: '极光色系樱花蜡粒',
    description: '白绿配色，适合各种场合',
    image: 'https://via.placeholder.com/100',
    price: 5.92,
    quantity: 1,
    selected: true
  }
])

const totalAmount = computed(() => {
  return cartItems.value.reduce((total, item) => {
    if (item.selected) {
      return total + (item.price * item.quantity)
    }
    return total
  }, 0)
})

const increaseQuantity = (index) => {
  cartItems.value[index].quantity++
}

const decreaseQuantity = (index) => {
  if (cartItems.value[index].quantity > 1) {
    cartItems.value[index].quantity--
  }
}

const removeItem = (index) => {
  cartItems.value.splice(index, 1)
}

const checkout = () => {
  const selectedItems = cartItems.value.filter(item => item.selected)
  if (selectedItems.length === 0) {
    alert('请至少选择一件商品进行结算')
    return
  }

  alert(`准备结算 ${selectedItems.length} 件商品，总价 ¥${totalAmount.value}`)
}
</script>

<style scoped>
.cart-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

.cart-header {
  text-align: center;
  margin-bottom: 20px;
}

.cart-header h1 {
  color: #ff4d4f;
  margin: 0;
}

.cart-items {
  margin-bottom: 20px;
}

.cart-item {
  display: flex;
  align-items: center;
  border-bottom: 1px solid #eee;
  padding: 15px 0;
}

.item-checkbox {
  width: 50px;
  display: flex;
  justify-content: center;
}

.item-image {
  width: 100px;
  height: 100px;
  margin: 0 20px;
}

.item-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 8px;
}

.item-info {
  flex: 1;
}

.item-info h3 {
  margin: 0 0 5px 0;
  color: #333;
}

.item-desc {
  color: #666;
  font-size: 14px;
  margin: 0 0 10px 0;
}

.item-price {
  color: #ff4d4f;
  font-weight: bold;
}

.item-actions {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}

.quantity-control {
  display: flex;
  align-items: center;
  gap: 10px;
}

.quantity-control button {
  width: 30px;
  height: 30px;
  background: #f5f5f5;
  border: 1px solid #ddd;
  cursor: pointer;
  border-radius: 50%;
}

.quantity-control span {
  width: 30px;
  text-align: center;
}

.item-buttons button {
  padding: 8px 16px;
  background: #f5f5f5;
  border: 1px solid #ddd;
  cursor: pointer;
  border-radius: 4px;
}

.checkout-area {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  background: #f9f9f9;
  border-radius: 8px;
}

.total-info {
  font-size: 18px;
  font-weight: bold;
  color: #333;
}

.checkout-btn {
  padding: 12px 40px;
  background: #ff4d4f;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.checkout-btn:hover {
  background: #e63a3a;
}
</style>