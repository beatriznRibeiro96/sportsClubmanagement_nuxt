<template>
    <div>
        <b-container>
            <b-row>
                <b-col lg="6" class="my-1">
                    <b-form-group
                        label="Filter"
                        label-cols-sm="3"
                        label-align-sm="right"
                        label-size="sm"
                        label-for="filterInput"
                        class="mb-0">
                        <b-input-group size="sm">
                            <b-form-input
                                v-model="filter"
                                type="search"
                                id="filterInput"
                                placeholder="Type to Search"></b-form-input>
                            <b-input-group-append>
                                <b-button :disabled="!filter" @click="filter = ''">Clear</b-button>
                            </b-input-group-append>
                        </b-input-group>
                    </b-form-group>
                </b-col>

                <b-col lg="6" class="my-1">
                    <b-form-group
                        label="Filter On"
                        label-cols-sm="3"
                        label-align-sm="right"
                        label-size="sm"
                        description="Leave all unchecked to filter on all data"
                        class="mb-0">
                            <b-form-checkbox-group v-model="filterOn" class="mt-1">
                            <b-form-checkbox value="id">Code</b-form-checkbox>
                            <b-form-checkbox value="description">Name</b-form-checkbox>
                            <b-form-checkbox value="categoryDescription">Category</b-form-checkbox>
                        </b-form-checkbox-group>
                    </b-form-group>
                </b-col>
            </b-row>
            <b-table bordered hover head-variant="dark" :items="products" :fields="fields" :filter="filter" :filterIncludedFields="filterOn" sort-icon-left>
                <template v-slot:cell(actions)="row">
                    <b-btn variant="info" :to="`/products/${row.item.id}`">DETAILS</b-btn>
                    <b-btn variant="warning" :to="`/products/edit/${row.item.id}`">EDIT</b-btn>
                    <b-btn variant="danger" @click="remove(row.item.id)">DELETE</b-btn>
                </template>
            </b-table>
            <b-btn variant="success" to="/products/create">Create a New Product</b-btn>
            <b-btn variant="secondary" to="/">Back</b-btn>
        </b-container>
    </div>
</template>
<script>
export default {
    data () {
        return {
            products: [],
            filter: null,
            filterOn: [],
            fields: [
                {
                    key: 'id',
                    label: 'Code',
                    numeric: true,
                    searchable: true
                },
                {
                    key: 'description',
                    label: 'Name',
                    searchable: true
                },
                {
                    key: 'price',
                    label: 'Price',
                    numeric: true
                },
                {
                    key: 'categoryDescription',
                    label: 'Category',
                    searchable: true
                },
                {
                    key: 'actions',
                    label: 'Actions'
                }
            ]
        }
    },
    methods: {
        getProducts() {
            this.$axios.$get('/api/products')
                .then((products) => {
                    this.products = products;
                })
                .catch((error) => {
                    console.log(error);
                })
        },
        remove(id) {
            this.$axios.$delete(`/api/products/${id}`)
                .then((product) => {
                    this.getProducts();
                })
                .catch((error) => {
                    console.log(error);
                })
        }
    },
    mounted() {
        this.getProducts();
    }
}
</script>