<template>
    <main>
        <div class="container-fluid c-section">
            <div class="row">
                <div class="col-sm-3"></div>
                <div class="col-sm-6">
                    <div class="a-spacing-medium">
                        <h2>Add a new Owner</h2>
                        <form>
                            <!-- Title -->
                            <div class="a-spacing-top-medium">
                                <label>Title</label>
                                <input
                                    class="a-input-text"
                                    style="width: 100%"
                                    v-model="title"
                                />
                            </div>
                            <!-- About-->
                            <div class="a-spacing-top-medium">
                                <label>About</label>
                                <input
                                    class="a-input-text"
                                    style="width: 100%"
                                    v-model="about"
                                />
                            </div>
                            <!--  Image  -->
                            <div class="a-spacing-top-medium">
                                <label>Onwer Image</label>
                                <div class="a-row a-spacing-top-medium">
                                    <label class="choosefile-button">
                                        <i class="fal fa-plus"></i>
                                        <input
                                            type="file"
                                            @change="onFileSelected"
                                        />
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
                                        <span
                                            class="a-button-text"
                                            @click="onAddOwner"
                                        >
                                            Add Owner
                                        </span>
                                    </span>
                                </span>
                            </div>
                        </form>
                        <br />

                        <ul class="list-group-item">
                            <li v-for="owner in Owners" :key="owner._id">
                                {{ owner.title }}
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
            let response = await $axios.$get("/api/owner/all");

            return {
                Owners: response.Owners,
            };
        } catch (error) {
            console.log(error);
        }
    },
    data() {
        return {
            title: "",
            about: "",
            selectedFile: null,
            fileName: "",
        };
    },
    methods: {
        onFileSelected(event) {
            this.selectedFile = event.target.files[0];
            this.fileName = event.target.files[0].name;
        },
        async onAddOwner() {
            try {
                let data = new FormData();
                data.append("title", this.title);
                data.append("about", this.about);
                data.append(
                    "ImageUrl",
                    this.selectedFile,
                    this.selectedFile.name
                );
                console.log(data);
                let response = await this.$axios.$post("/api/owner/add", data);

                this.$router.push("/owner");
            } catch (error) {
                console.log(error);
            }
        },
    },
};
</script>
