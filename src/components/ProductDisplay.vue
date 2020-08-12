<template>
  <div class="product-display">
    <div class="product-container">
      <div class="product-image">
        <img :src="image" alt />
      </div>
      <div class="product-info">
        <h1>{{ title }}</h1>

        <p v-if="inStock">In Stock</p>
        <!-- <p v-else-if="inventory <= 10 && inventory > 0">Almost sold out!</p> -->
        <p v-else>Out of Stock</p>

        <p>Shipping: {{shipping}}</p>

        <ul>
          <li v-for="detail in details" :key="detail">{{detail}}</li>
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
          @click="addToCart"
          :disabled="!inStock"
          :class="{ disabledButton: !inStock }"
        >Add to Cart</button>
      </div>
    </div>
  </div>
  <ReviewList v-if="reviews.length > 0" :reviews="reviews" />
  <ReviewForm @review-submitted="addReview" />
</template>

<script>
import ReviewForm from "./ReviewForm.vue";
import ReviewList from "./ReviewList.vue";

export default {
  name: "ProductDisplay",
  props: {
    premium: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      brand: "Alexandrite",
      product: "Socks",
      selectedVariant: 0,
      details: ["50% cotton", "30% wool", "20% polyester"],
      variants: [
        {
          id: 2234,
          color: "green",
          image: "/src/assets/socks_green.jpg",
          quantity: 50,
        },
        {
          id: 2235,
          color: "blue",
          image: "/src/assets/socks_blue.jpg",
          quantity: 0,
        },
      ],
      reviews: [],
    };
  },
  methods: {
    addToCart() {
      this.$emit("add-to-cart", this.variants[this.selectedVariant].id);
    },
    updateVariant(index) {
      this.selectedVariant = index;
    },
    addReview(review) {
      this.reviews.push(review);
    },
  },
  computed: {
    title() {
      return this.brand + " " + this.product;
    },
    image() {
      return this.variants[this.selectedVariant].image;
    },
    inStock() {
      return this.variants[this.selectedVariant].quantity;
    },
    shipping() {
      return this.premium ? "Free" : 2.99;
    },
  },
  components: {
    ReviewForm,
    ReviewList,
  },
  emits: ["add-to-cart"],
};
</script>