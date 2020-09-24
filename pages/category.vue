<template>
    <main>
        <div class="container-fluid c-section">
            <div class="row">
                <div class="col-sm-3"></div>
                <div class="col-sm-6">
                    <div class="a-spacing-medium">
                        <h2>Add a new Category</h2>
                        <form>
                            <div class="a-spacing-top-medium">
                                <label>Type</label>
                                <input
                                    class="a-input-text"
                                    style="width: 100%"
                                    v-model="categoryType"
                                />
                            </div>
                            <!-- Button -->

                            <hr />
                            <div class="a-spacing-top-large">
                                <span class="a-button-register">
                                    <span class="a-button-inner">
                                        <span
                                            class="a-button-text"
                                            @click="onAddCategory"
                                        >
                                            Add Category
                                        </span>
                                    </span>
                                </span>
                            </div>
                        </form>
                        <br />

                        <ul class="list-group-item">
                            <li
                                v-for="category in Categories"
                                :key="category._id"
                            >
                                {{ category.categoryType }}
                            </li>
                        </ul>
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
            let response = await $axios.$get("/api/category/all");

            return {
                Categories: response.Categories,
            };
        } catch (error) {
            console.log(error);
        }
    },
    data() {
        return {
            categoryType: "",
        };
    },
    methods: {
        async onAddCategory() {
            try {
                let data = { categoryType: this.categoryType };
                let response = await this.$axios.$post(
                    "/api/category/add",
                    data
                );

                if (response.status) {
                    this.categories.push(data);
                }
            } catch (error) {
                console.log(error);
            }
        },
    },
};
</script>
