<template>
  <div class="product-display">
    <div class="product-container">
      <div class="product-image">
        <img :src="image" />
      </div>
      <div class="product-info">
        <h1>{{ title }}</h1>
        <p v-if="inStock">In Stock</p>
        <p v-else>Out of Stock</p>
        <p>Shipping: {{ shipping }}</p>
        <ul>
          <li v-for="(detail, index) in details" :key="index">{{ detail }}</li>
        </ul>
        <div
          v-for="(variant, index) in variants"
          :key="variant.id"
          @mouseover="updateVariant(index)"
          class="color-circle"
          :style="{ backgroundColor: variant.color }"
        ></div>

        <button
          class="button"
          :class="{ disabledButton: !inStock }"
          :disabled="!inStock"
          @click="addToCart"
        >
          Add to Cart
        </button>
      </div>
    </div>
    <review-list v-if="reviews.length" :reviews="reviews"></review-list>
    <review-form @review-submitted="addReview"></review-form>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed, SetupContext } from "vue";
import ReviewList from "./ReviewList.vue";
import ReviewForm from "./ReviewForm.vue";

export type ReviewType = {
  name: string;
  review: string;
  rating: number | null;
};

export default defineComponent({
  name: "ProductDisplay",
  components: {
    ReviewList,
    ReviewForm
  },
  props: {
    premium: {
      type: Boolean,
      required: true
    }
  },
  emits: ["add-to-cart"],
  setup(props, context: SetupContext) {
    const product = "Socks";
    const brand = "Vue Mastery";
    const selectedVariant = ref(0);
    const details = ["50% cotton", "30% wool", "20% polyester"];
    const variants = [
      {
        id: 2234,
        color: "green",
        image: require("@/assets/images/socks_green.jpg"),
        quantity: 50
      },
      {
        id: 2235,
        color: "blue",
        image: require("@/assets/images/socks_blue.jpg"),
        quantity: 0
      }
    ];
    const reviews = ref<ReviewType[]>([]);

    const title = computed(() => `${brand} ${product}`);
    const image = computed(() => variants[selectedVariant.value].image);
    const inStock = computed(() => variants[selectedVariant.value].quantity);
    const shipping = computed(() => {
      if (props.premium) {
        return "Free";
      }
      return 2.99;
    });

    const addToCart = () =>
      context.emit("add-to-cart", variants[selectedVariant.value].id);
    const updateVariant = (index: number) => (selectedVariant.value = index);
    const addReview = (review: ReviewType) => reviews.value.push(review);

    return {
      // data
      product,
      brand,
      selectedVariant,
      details,
      variants,
      reviews,
      // computed
      title,
      image,
      inStock,
      shipping,
      // methods
      addToCart,
      updateVariant,
      addReview
    };
  }
});
</script>

<style>
.color-circle {
  width: 50px;
  height: 50px;
  margin-top: 8px;
  border: 2px solid #d8d8d8;
  border-radius: 50%;
}

.product-display {
  display: flex;
  flex-direction: column;
  padding: 1rem;
}

.product-container {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}

.product-image,
.product-info {
  width: 50%;
}
</style>
