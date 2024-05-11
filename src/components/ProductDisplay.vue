<template>
    <div class="product-display">

        <div class="product-container">
            <div class="product-image">
                <img :src="image" alt="A pair of socks">
            </div>
            <div class="product-info">
                <h1>{{ title }}</h1>
                <p>Shippping {{ shipping }}</p>
                <p v-show="onSale">On sale!</p>
                <p v-if="inventory > 10">In stock</p>
                <p v-else-if="inventory <= 10 && inventory > 0">Almost sold out!</p>
                <p v-else>Out of stock</p>
                <ul>
                    <li v-for="(detail, detailIndex) in details" :key="detailIndex">
                        {{ detail }}
                    </li>
                </ul>
                <div v-for="(variant, variantIndex) in variants" :key="variant.id"
                    @mouseover.stop="updateVariant(variantIndex)" class="color-circle"
                    :style="{ backgroundColor: variant.color }">
                </div>
                <ul>
                    <li v-for="(size, sizeIndex) in sizes" :key="sizeIndex">
                        {{ size }}
                    </li>
                </ul>
            </div>

        </div>
        <button class="button" @click="addToCart" :disabled="inventory === 0"
            :class="{ disabledButton: inventory === 0 }">Add to cart</button>
        <button class="button" @click="deleteFromCart">Delete from cart</button>
    </div>

    <ReviewList v-if="variants[selectedVariant].reviews.length" :reviews="variants[selectedVariant].reviews" />
    <ReviewForm @review-submitted="addReview" />
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import ReviewForm from '@/components/ReviewForm.vue';
import ReviewList from '@/components/ReviewList.vue'

const props = defineProps({
    premium: {
        type: Boolean,
        required: true,
    },
})

const emits = defineEmits(['add-to-cart', 'delete-from-cart'])

const product = ref('Socks')
const selectedVariant = ref(0)
const brand = ref('Vue Mastery')

const details = ref([
    '50% cotton',
    '30% wool',
    '20% polyester',
])
const variants = ref<{ id: number, color: string, image: string, quantity: number, onSale: boolean, reviews: {}[] }[]>([
    {
        id: 2234,
        color: 'green',
        image: '/assets/images/socks_green.jpg',
        quantity: 50,
        onSale: true,
        reviews: [],
    },
    {
        id: 2235,
        color: 'blue',
        image: '/assets/images/socks_blue.jpg',
        quantity: 8,
        onSale: false,
        reviews: [],
    }
])
const sizes = ref([
    'small',
    'medium',
    'large',
])

function addToCart() {
    emits('add-to-cart', variants.value[selectedVariant.value].id)
}

function deleteFromCart() {
    emits('delete-from-cart', variants.value[selectedVariant.value].id)
}

function updateVariant(index: number) {
    selectedVariant.value = index
}

function addReview(review: object) {
    variants.value[selectedVariant.value].reviews.push(review)
}

const title = computed(() => product.value + ' - ' + brand.value)
const image = computed(() => variants.value[selectedVariant.value].image)
const inventory = computed(() => variants.value[selectedVariant.value].quantity)
const onSale = computed(() => variants.value[selectedVariant.value].onSale)
const shipping = computed(() => props.premium ? 'Free' : '2.99')

</script>