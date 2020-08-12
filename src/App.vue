<template>
  <div class="nav-bar"></div>
  <div class="cart">Cart({{ cart.length }})</div>
  <ProductDisplay :premium="premium" @add-to-cart="updateCart" />

</template>

<script>
import ProductDisplay from './components/ProductDisplay.vue';

export default {
  name: "App",
  data() {
    return {
      cart: [],
      premium: true,
    };
  },
  methods: {
    updateCart(id) {
      this.cart.push(id);
    }
  },

  components: {
    ProductDisplay,
  }
};
</script>
