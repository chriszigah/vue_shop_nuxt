<template>
  <main>
    <div class="a-spacing-large">
      <div class="container-fluid browsing-history">
        <div class="row">
          <div class="col-sm-8 col-8">
            <h1 class="a-size-large a-spacing-none a-text-normal">
              All Products
            </h1>
            <div class="a-spacing-large"></div>
            <!-- Buttons -->
            <nuxt-link to="/product" class="a-button-buy-again"
              >Add a new Product</nuxt-link
            >
            <nuxt-link to="/category" class="a-button-history margin-right-10"
              >Add a new Category</nuxt-link
            >

            <nuxt-link to="owner" class="a-button-history margin-right-10"
              >Add a new Owner</nuxt-link
            >

            <!-- End of Buttons -->
          </div>
        </div>
      </div>
      <!-- Listing Page -->
      <div class="a-spacing-large"></div>
      <div class="container-fluid browsing-history">
        <div class="row">
          <div
            v-for="(product, index) in products"
            :key="product._id"
            class="col-xl-2 col-lg-2 col-md-3 col-sm-6 col-6 br bb"
          >
            <div class="history-box">
              <!-- Product Image -->
              <a href="#" class="img-fluid">
                <img :src="product.imageUrl" class="img-fluid" />
              </a>
              <div class="a-spacing-top-base asin-title">
                <span class="a-text-normal">
                  <div class="p13n-sc-truncated">{{ product.title }}</div>
                </span>
              </div>
              <!-- Product Rating -->
              <div class="a-row">
                <i class="fas fa-star"></i>
                <i class="fas fa-star"></i>
                <i class="fas fa-star"></i>
                <i class="fas fa-star"></i>
                <i class="fas fa-star"></i>
                <i class="fas fa-star"></i>
                <span class="a-letter-space"></span>
                <span class="a-color-tertiary a-size-small asin-reviews"
                  >(1732)</span
                >
              </div>
              <!-- Product Price -->
              <div class="a-row">
                <span class="a-size-base a-color-price"></span>
                <span class="p13n-sc-price">GH¢ {{ product.price }}</span>
              </div>
              <!-- Product Buttons -->

              <div class="row">
                <nuxt-link
                  :to="`/product/${product._id}`"
                  class="a-button-history margin-right-10"
                  >Upadate</nuxt-link
                >
                <button
                  @click="onDeleteProduct(product._id, index)"
                  class="a-button-history margin-right-10"
                >
                  Delete
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- End of Listing Page -->
    </div>
  </main>
</template>

<script>
export default {
  // asyncDat is fetching data before nuxt page finished loading in the browser.
  // it is good for SEO because the data will be loaded first
  async asyncData({ $axios }) {
    try {
      let response = await $axios.$get("/api/product/all");

      return {
        products: response.Products,
      };
    } catch (error) {
      console.log(error);
    }
  },

  methods: {
    async onDeleteProduct(id, index) {
      try {
        let response = await this.$axios.$delete(`/api/product/${id}`);

        if (response.status === 200) {
          this.product.splice(index, 1);
        }
      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
