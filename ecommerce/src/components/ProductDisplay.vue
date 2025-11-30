<template>
  <div :class="sectionClass" class="section-container">
    <!-- Available Product -->
    <div v-if="product && isAvailable" class="product-card">
      <img :src="product.image" class="product-img" />

      <div class="product-info">
        <h2>{{ product.title }}</h2>

        <div class="category-rating">
          <p>{{ product.category }}</p>
          <div class="rating-dots">
            <span v-for="n in 5" :key="n" class="dot"></span>
          </div>
        </div>

        <p class="description">{{ product.description }}</p>

        <h3 class="price">${{ product.price }}</h3>

        <div class="button-row">
          <button class="buy-btn">Buy now</button>
          <button class="next-btn" @click="nextProduct">Next product</button>
        </div>
      </div>
    </div>

    <!-- Unavailable Product -->
    <div v-else class="unavailable-box">
      <div class="sad-face">:(</div>
      <p>This product is unavailable to show</p>
      <button class="next-unavailable-btn" @click="nextProduct">
        Next product
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      index: 1,
      product: null,
      isAvailable: false,
    };
  },

  computed: {
    sectionClass() {
      if (!this.product) return "";

      if (this.product.category === "men's clothing") return "men-section";
      if (this.product.category === "women's clothing") return "women-section";
      return "unavailable-section";
    },
  },

  methods: {
    async nextProduct() {
      if (this.index > 20) this.index = 1;

      const url = `https://fakestoreapi.com/products/${this.index}`;
      const res = await fetch(url);
      const data = await res.json();

      if (
        data.category === "men's clothing" ||
        data.category === "women's clothing"
      ) {
        this.product = data;
        this.isAvailable = true;
      } else {
        this.product = data;
        this.isAvailable = false;
      }

      this.index++;
    },
  },

  mounted() {
    this.nextProduct();
  },
};
</script>