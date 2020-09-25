<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-large"></div>
            <h2 style="text-align: center">Add a new Product</h2>
            <form>
              <!-- Category Dropdown -->
              <div class="a-spacing-top-medium">
                <label>Category</label>
                <select class="a-select-option" v-model="categoryID">
                  <option v-for="category in categories" value="category._id">{{
                    category.categoryType
                  }}</option>
                </select>
              </div>
              <!--  Owner Dropdown-->
              <div class="a-spacing-top-medium">
                <label>Owner</label>
                <select class="a-select-option" v-model="ownerID">
                  <option v-for="owner in owners" value="owner._id">{{
                    owner.title
                  }}</option>
                </select>
              </div>

              <!--  Title -->
              <div class="a-spacing-top-medium">
                <label>Title</label>
                <input
                  type="text"
                  class="a-input-text"
                  style="width: 100%"
                  v-model="title"
                />
              </div>

              <!--  Description  -->
              <div class="a-spacing-top-medium">
                <label>Description</label>
                <textarea
                  placeholder="Provide details such as Product details"
                  style="width: 100%"
                  v-model="description"
                ></textarea>
              </div>

              <!--  Price  -->
              <div class="a-spacing-top-medium">
                <label>Price</label>
                <input
                  type="number"
                  class="a-input-text"
                  style="width: 100%"
                  v-model="price"
                />
              </div>

              <!--  Stock Quantity  -->
              <div class="a-spacing-top-medium">
                <label>Stock Quantity</label>
                <input
                  type="number"
                  class="a-input-text"
                  style="width: 100%"
                  v-model="stockQuantity"
                />
              </div>

              <!--  Image  -->
              <div class="a-spacing-top-medium">
                <label>Product Image</label>
                <div class="a-row a-spacing-top-medium">
                  <label class="choosefile-button">
                    <i class="fal fa-plus"></i>
                    <input type="file" @change="onFileSelected" />
                  </label>
                  <p style="margin-top: 2px">
                    {{ fileName }}
                  </p>
                </div>
              </div>
              <!-- Button -->
              <hr />
              <div class="a-spacing-top-large">
                <span class="a-button-register">
                  <span class="a-button-inner">
                    <span class="a-button-text" @click="onAddProduct"
                      >Add Product</span
                    >
                  </span>
                </span>
              </div>
            </form>
          </div>
        </div>
        <div class="col-sm-3"></div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    try {
      let categories = $axios.$get("/api/category/all");
      let owners = $axios.$get("/api/owner/all");

      const [catResponse, ownerResponse] = await Promise.all([
        categories,
        owners,
      ]);

      return {
        categories: catResponse.Categories,
        owners: ownerResponse.Owners,
      };
    } catch (err) {
      console.log(err);
    }
  },
  data() {
    return {
      categoryID: null,
      ownerID: null,
      title: "",
      price: 0,
      stockQuantity: 0,
      description: "",
      selectedFile: null,
      fileName: "",
    };
  },
  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0];
      this.fileName = event.target.files[0].name;
    },
    async onAddProduct() {
      let data = new FormData();
      data.append("title", this.title);
      data.append("description", this.description);
      data.append("categoryID", this.categoryID);
      data.append("ownerID", this.ownerID);
      data.append("stockQuantity", this.stockQuantity);
      data.append("price", this.price);
      data.append("ImageUrl", this.selectedFile, this.selectedFile.name);

      let result = await this.$axios.$post("/api/product/add", data);

      this.$router.push("/");
    },
  },
};
</script>
