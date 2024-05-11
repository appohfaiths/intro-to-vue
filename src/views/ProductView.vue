<template>
    <div class="cart">Cart ({{ cart.length }})</div>
    <ProductDisplay :premium="premium" @add-to-cart="addToCart" @delete-from-cart="deleteFromCart" />
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import ProductDisplay from '@/components/ProductDisplay.vue'

const premium = ref(true)

const cart = ref<number[]>([])

function addToCart(productId: number) {
    cart.value.push(productId)
}

function deleteFromCart(productId: number) {
    cart.value = cart.value.filter((id) => id !== productId)
}
</script>