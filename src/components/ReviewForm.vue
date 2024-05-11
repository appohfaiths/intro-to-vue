<template>
    <form class="review-form" @submit.prevent="onSubmit">
        <h3>Leave a review</h3>
        <label for="name">Name:</label>
        <input type="text" id="name" v-model="reviewForm.name">

        <label for="review">Review:</label>
        <textarea name="review" id="review" v-model="reviewForm.review"></textarea>

        <label for="rating">Rating:</label>
        <select name="rating" id="rating" v-model.number="reviewForm.rating">
            <option :value="null">Select a rating</option>
            <option value="5">5</option>
            <option value="4">4</option>
            <option value="3">3</option>
            <option value="2">2</option>
            <option value="1">1</option>
        </select>

        <button type="submit" class="button">Submit</button>
    </form>
</template>

<script lang="ts" setup>
import { ref } from 'vue';

const emits = defineEmits(['review-submitted'])

const reviewForm = ref({
    name: '',
    review: '',
    rating: '',
})

const onSubmit = () => {
    if (reviewForm.value.name === '' || reviewForm.value.review === '' || reviewForm.value.rating === null) {
        alert('Review is incomplete. Please fill out all fields')
        return;
    }
    emits('review-submitted', reviewForm.value);
    reviewForm.value.name = "";
    reviewForm.value.review = "";
    reviewForm.value.rating = "";
}
</script>