<template>
    <div class="review-container">
        <h3>Reviews:</h3>
        <ul>
            <li v-for="(review, reviewIndex) in reviews" :key="reviewIndex">
                <p>{{ review.name }}</p>
                <p>{{ review.review }}</p>
                <p>{{ review.rating }}</p>
            </li>
        </ul>
    </div>
</template>

<script lang="ts" setup>
defineProps({
    reviews: {
        type: Array,
        required: true
    }
})
</script>