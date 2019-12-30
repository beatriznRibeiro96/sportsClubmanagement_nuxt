<template>
    <b-container>
        <h2>Edit Product - {{ product.description }}</h2>
        <div v-if="error == null">
            <b-form @submit.prevent="edit(product.id)">
                <b-form-group label="Description" description="Product's name">
                    <b-input
                        name="description"
                        type="text"
                        placeholder="Name"
                        v-model.trim="product.description"
                        required />
                </b-form-group>

                <b-row align-v="center">
                    <b-col cols="12" md="auto">
                        <b-form-group label="Price" description="Product's price">
                            <b-input
                                name="price"
                                type="number"
                                v-model="product.price"
                                step=".01"
                                required />
                        </b-form-group>
                    </b-col>
                </b-row>

                <b-row align-v="center">
                    <b-col cols="12" md="auto">
                        <b-form-group label="Category" description="Product type">
                            <b-select
                                name="categoryDescription"
                                v-model="product.categoryID"
                                :options="categories"
                                value-field="id" 
                                text-field="description" />
                            <template v-slot:first>
                                <option :value="null" disabled>-- Please select an option --</option>
                            </template>
                        </b-form-group>
                    </b-col>
                    <b-col style="margin-bottom:0.45rem">
                        <b-btn variant="success" @click.prevent="newCategory">Create a New Category</b-btn>
                        <b-btn variant="warning" @click.prevent="editCategoty">Edit</b-btn>
                        <b-btn variant="danger" @click.prevent="deleteCategoty">Delete</b-btn>
                    </b-col>
                </b-row>

                <b-btn variant="secondary" to="/products">RETURN</b-btn>
                <b-button type="submit" variant="primary">EDIT</b-button>
                <b-btn variant="warning" @click.prevent="edit">RESET</b-btn>
            </b-form>
            <b-alert class="mt-3" :variant="message.variant" dismissible :show="message.show">
                {{message.text}}
            </b-alert>
        </div>
        <div v-else>
            <b-alert variant="danger" show>{{ error }}</b-alert>
        </div>
    </b-container>
</template>
<script>
    export default {
        data() {
            return {
                product: {},
                categories: {},
                error: null,
                message: { text: '', variant: '', show: false }
            }
        },
        methods: {
            getProduct() {
                this.$axios.$get(`/api/products/${this.id}`)
                    .then((product) => {
                        this.product = product;
                    })
                    .catch((error) => {
                        this.error = error;
                        console.log(error);
                    })
            },
            getCategories() {
                this.$axios.$get('/api/categories/')
                    .then((categories) => {
                        this.categories = categories;
                    })
                    .catch((error) => {
                        this.error = error;
                        console.log(error);
                    })
            },
            edit(id) {
                if(Number(this.product.price) <= 0.00) {
                    this.message.variant = 'danger';
                    this.message.text = 'Invalid price';
                    this.message.show = true;
                    return;
                }

                this.$axios.$put(`/api/products/${id}`, this.product)
                    .then((product) => {
                        this.$router.push(`/products/edit/${product.id}`)

                        this.message.variant = 'success';
                        this.message.text = 'Product edited with success';
                        this.message.show = true;
                    })
                    .catch((error) => {
                        this.error = error;
                        console.log(error);
                    })
                
            },
            reset() {
                this.getProduct();
            },
            newCategory() {
                let category = prompt("New category:");

                if(category != null && category.length > 0) {
                    this.$axios.$post('/api/categories/', {
                            description: category
                        })
                        .then((category) => {
                            this.getCategories();
                            this.product.categoryID = category.id;

                            this.message.variant = 'success';
                            this.message.text = 'Category created with success';
                            this.message.show = true;
                        })
                        .catch((error) => {
                            this.error = error;
                            console.log(error);
                        })
                } else {
                    this.message.variant = 'danger';
                    this.message.text = 'Invalid Category';
                    this.message.show = true;
                }
            },
            editCategoty() {
                let category = prompt("Edit category:");

                if(category != null && category.length > 0) {
                    this.$axios.$put(`/api/categories/${this.product.categoryID}`, {
                            description: category
                        })
                        .then((category) => {
                            this.getCategories();
                            this.product.categoryID = category.id;

                            this.message.variant = 'success';
                            this.message.text = 'Category edited with success';
                            this.message.show = true;
                        })
                        .catch((error) => {
                            this.error = error;
                            console.log(error);
                        })
                } else {
                    this.message.variant = 'danger';
                    this.message.text = 'Invalid Category';
                    this.message.show = true;
                }
            },
            deleteCategoty() {
                if (confirm('Deleting this category it will invalidated all products inside.')) {
                    this.$axios.$delete(`/api/categories/${this.product.categoryID}`)
                        .then((category) => {
                            this.$router.go(-1);
                        })
                        .catch((error) => {
                            console.log(error);
                        })
                }
            }
        },
        computed: {
            id() {
                return this.$route.params.code
            }
        },
        mounted() {
            this.getProduct();
            this.getCategories();
        }
    }
</script>
