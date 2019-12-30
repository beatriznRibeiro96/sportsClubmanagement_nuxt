<template>
  <b-container>
    <h2>Product Details</h2>
    <div v-if="error == null">
        <p><strong>Code:</strong> {{ product.id }}</p>
        <p><strong>Name:</strong> {{ product.description }}</p>
        <p><strong>Category:</strong> {{ product.categoryDescription }}</p>
        <p><strong>Price:</strong> {{ product.price }} €</p>
        <b-btn variant="secondary" to="/products">Back</b-btn>
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
            error: null
        }
    },
    computed: {
        id() {
            return this.$route.params.code
        }
    },
    created() {
        this.$axios.$get(`/api/products/${this.id}`)
            .then((product) => {
                this.product = product;
            })
            .catch((error) => {
                this.error = error;
                console.log(error);
            })
    }
}
</script>