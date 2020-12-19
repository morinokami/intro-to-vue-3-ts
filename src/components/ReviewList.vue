<template>
  <div class="review-container">
    <h3>Reviews:</h3>
    <ul>
      <li v-for="(review, index) in reviews" :key="index">
        {{ review.name }} gave this {{ review.rating }} stars
        <br />
        "{{ review.review }}"
        <br />
        Recommended: {{ review.recommend }}
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";
import { ReviewType } from "./ProductDisplay.vue";

export default defineComponent({
  name: "ReviewList",
  props: {
    reviews: {
      type: Array as PropType<ReviewType[]>,
      required: true
    }
  }
});
</script>

<style>
.review-container {
  width: 425px;
  padding: 20px;
  background-color: white;
  -webkit-box-shadow: 0px 2px 20px -12px rgba(0, 0, 0, 0.57);
  -moz-box-shadow: 0px 2px 20px -12px rgba(0, 0, 0, 0.57);
  box-shadow: 2px 20px -12px rgba(0, 0, 0, 0.57);
  margin-left: 40px;
  border: 2px solid #d8d8d8;
}

.review-container li {
  margin-bottom: 30px;
}
</style>
