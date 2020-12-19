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
  >
</template>

<script lang="ts">
import { defineComponent } from "vue";
import ReviewList from "./ReviewList.vue";
import ReviewForm from "./ReviewForm.vue";

export type VariantType = {
  id: number;
  color: string;
  image: string;
  quantity: number;
};

export type ReviewType = {
  name: string;
  review: string;
  rating: number | null;
};

export type DataType = {
  product: string;
  brand: string;
  selectedVariant: number;
  details: string[];
  variants: VariantType[];
  reviews: ReviewType[];
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
  data(): DataType {
    return {
      product: "Socks",
      brand: "Vue Mastery",
      selectedVariant: 0,
      details: ["50% cotton", "30% wool", "20% polyester"],
      variants: [
        {
          id: 2234,
          color: "green",
          image: require("@/assets/images/socks_green.jpg"),
          quantity: 50
        },
        {
          id: 2235,
          color: "blue",
          image: require("../assets/images/socks_blue.jpg"),
          quantity: 0
        }
      ],
      reviews: []
    };
  },
  methods: {
    addToCart() {
      this.$emit("add-to-cart", this.variants[this.selectedVariant].id);
    },
    updateVariant(index: number) {
      this.selectedVariant = index;
    },
    addReview(review: ReviewType) {
      this.reviews.push(review);
    }
  },
  computed: {
    title(): string {
      return this.brand + " " + this.product;
    },
    image(): string {
      return this.variants[this.selectedVariant].image;
    },
    inStock(): number {
      return this.variants[this.selectedVariant].quantity;
    },
    shipping(): string | number {
      if (this.premium) {
        return "Free";
      }
      return 2.99;
    }
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
