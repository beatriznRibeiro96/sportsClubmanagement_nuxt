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
                            <b-form-checkbox value="description">Name</b-form-checkbox>
                            <b-form-checkbox value="categoryDescription">Category</b-form-checkbox>
                        </b-form-checkbox-group>
                    </b-form-group>
                </b-col>
            </b-row>
            <b-btn variant="success" @click.prevent="newOrder">Create Order</b-btn>
            <b-table bordered hover head-variant="dark" 
                    :items="products" 
                    :fields="fields" :filter="filter" :filterIncludedFields="filterOn" 
                    :select-mode="multi" @row-selected="onRowSelected" selectable sort-icon-left>
                <template v-slot:cell(selected)="{ rowSelected }">
                    <template v-if="rowSelected">
                        <span aria-hidden="true">&check;</span>
                        <span class="sr-only">Selected</span>
                    </template>
                    <template v-else>
                        <span aria-hidden="true">&nbsp;</span>
                        <span class="sr-only">Not selected</span>
                    </template>
                </template>
                <template v-slot:cell(quantity)="row">
                    <b-input
                        type="number"
                        v-model="row.item.quantity"
                        step="1"
                        min="1"
                        required />
                </template>
            </b-table>
            Selected Rows:<br>
            {{ selected }}
            <b-alert variant="danger" v-if="error != null">{{ error }}</b-alert>
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
                    key: 'selected',
                    label: 'Selected',
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
                    key: 'quantity',
                    label: 'Quantity',
                    numeric: true
                }
            ],
            itemList: {productID: 0, quantity: 0},
            selected: [],
            error: null            
        }
    },
    methods: {
        getProducts() {
            this.$axios.$get('/api/products')
                .then((products) => {
                    products.forEach(element => {
                        this.products.push({
                            productID: element.id,
                            description: element.description,
                            price: element.price,
                            categoryDescription: element.categoryDescription,
                            quantity: 0
                        })
                    });
                })
                .catch((error) => {
                    console.log(error);
                })
        },
        onRowSelected(items) {
            items.forEach(item => {
                this.products.forEach(product => {                    
                    if(product.productID == item.productID && product.quantity == 0) {
                        product.quantity = 1;
                    }
                });
            });
            this.selected = items
        },
        newOrder() {
            if(this.selected.length == 0 || this.selected == null) {
                this.error = 'No items selected';
                return;
            }
            console.log(this.error);

            let order = {
                username: this.$auth.user.sub,
                lineItemOrders: []
            };

            this.selected.forEach(element => {
                order.lineItemOrders.push({ productID: element.productID, quantity: element.quantity})
            });

            this.$axios.$post('/api/orders/', order)
                    .then((product) => {
                        this.$router.push('/orders/')
                    })
                    .catch((error) => {
                        this.error = error;
                        console.log(error);
                    })
        }
    },
    mounted() {
        this.getProducts();
    }
}
</script>