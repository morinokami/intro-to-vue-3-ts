<template>
  <form class="review-form" @submit.prevent="onSubmit">
    <h3>Leave a review</h3>
    <label for="name">Name:</label>
    <input id="name" v-model="name" />
    <label for="review">Review:</label>
    <textarea id="review" v-model="review"></textarea>
    <label for="rating">Rating:</label>
    <select id="rating" v-model.number="rating">
      <option>5</option>
      <option>4</option>
      <option>3</option>
      <option>2</option>
      <option>1</option>
    </select>
    <label for="recommend">Would you recommend this product?</label>
    <select id="recommend" v-model="recommend">
      <option>Yes</option>
      <option>No</option>
    </select>
    <input class="button" type="submit" value="Submit" />
  </form>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export type DataType = {
  name: string;
  review: string;
  rating: number | null;
  recommend: string | null;
};

export default defineComponent({
  name: "ReviewForm",
  emits: ["review-submitted"],
  data(): DataType {
    return {
      name: "",
      review: "",
      rating: null,
      recommend: null
    };
  },
  methods: {
    onSubmit() {
      if (
        this.name === "" ||
        this.review === "" ||
        this.rating === null ||
        this.recommend === null
      ) {
        alert("Review is incomplete. Please fill out every field.");
        return;
      }

      const productReview = {
        name: this.name,
        review: this.review,
        rating: this.rating,
        recommend: this.recommend
      };
      this.$emit("review-submitted", productReview);

      this.name = "";
      this.review = "";
      this.rating = null;
      this.recommend = null;
    }
  }
});
</script>

<style>
.review-form {
  display: flex;
  flex-direction: column;
  width: 425px;
  padding: 20px;
  margin: 40px;
  border: 2px solid #d8d8d8;
  background-color: white;
  -webkit-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  -moz-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  box-shadow: 2px 15px -12px rgba(0, 0, 0, 0.57);
}

.review-form .button {
  display: block;
  margin: 30px auto;
}
</style>
