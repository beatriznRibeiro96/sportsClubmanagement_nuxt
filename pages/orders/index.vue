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
                            <b-form-checkbox value="status">Status</b-form-checkbox>
                        </b-form-checkbox-group>
                    </b-form-group>
                </b-col>
            </b-row>
            <b-table bordered hover head-variant="dark" :items="orders" :fields="fields" :filter="filter" :filterIncludedFields="filterOn" sort-icon-left>
            <template v-slot:cell(priceTotal)="row">
                {{ row.item.priceTotal.toFixed(2)}}
            </template>
                <template v-slot:cell(missing)="row">
                {{ row.item.missing.toFixed(2)}}
            </template>
            <template v-slot:cell(actions)="row">
                <b-button size="sm" @click="row.toggleDetails" class="mr-2">
                    {{ row.detailsShowing ? 'Hide' : 'Show'}} Details
                </b-button>
            </template>
            <template v-slot:row-details="row">
                <b-card>
                    <b-row class="mb-2">
                        <b-col sm="3" class="text-sm-right"><b>Content:</b></b-col>
                        <b-col>
                            <li v-for="item in row.item.lineItemOrders">
                                <strong>Item: </strong>{{ item.productDescription }},
                                <strong>Quantity: </strong>{{ item.quantity }},
                                <strong>Price: </strong>{{ item.priceQuantity }} 
                            </li>
                        </b-col>
                    </b-row>
                    <hr>
                    <b-row class="mb-2">
                        <b-col sm="3" class="text-sm-right"><b>Payments:</b></b-col>
                        <b-col>
                            <li v-for="item in row.item.payments">
                                <strong>Amount: </strong>{{ item.amount }}
                            </li>
                        </b-col>
                    </b-row>
                </b-card>
            </template>
            </b-table>
            <b-btn variant="success" to="/products/create">Create a New Order</b-btn>
            <b-btn variant="secondary" to="/">Back</b-btn>
        </b-container>
    </div>
</template>
<script>
export default {
    data () {
        return {
            orders: [],
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
                    key: 'status',
                    label: 'Status',
                    searchable: true
                },
                {
                    key: 'priceTotal',
                    label: 'Total',
                    numeric: true,
                },
                {
                    key: 'missing',
                    label: 'Missing',
                    numeric: true
                },
                {
                    key: 'actions',
                    label: 'Actions'
                }
            ]
        }
    },
    methods: {
        getOrders() {
            this.$axios.$get(`/api/orders/${this.$auth.user.sub}`)
                .then((orders) => {
                    this.orders = orders;
                    this.orders.priceTotal.toFixed(2);
                    this.orders.missing.toFixed(2);
                })
                .catch((error) => {
                    console.log(error);
                })
        }
    },
    mounted() {
        this.getOrders();
    }
}
</script>