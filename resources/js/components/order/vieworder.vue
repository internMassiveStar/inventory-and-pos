<template>
    <div>
        <div class="row justify-content-center">
            <div class="col-xl-12 col-lg-12 col-md-12">
                <router-link to="/order" class="btn btn-primary btn-sm">Go Back</router-link>
                <div class="card shadow-sm my-5">
                    <div class="card-body p-0">
                        <div class="row">
                            <div class="col-lg-12 mx-auto">
                                <div class="login-form">
                                    <div class="text-center">
                                        <h1 class="h4 text-gray-900 mb-4">Order Details</h1>
                                    </div>
                                     <div class="row">
                                        <div class="col-lg-6 mb-4">
                                            <!-- Simple Tables -->
                                            <div class="card">
                                                <div class="card-header py-3 d-flex flex-row align-items-center justify-content-between">
                                                    <h6 class="m-0 font-weight-bold text-primary">Order Details</h6>
                                                </div>
                                                <div class="table-responsive">
                                                    <ul class="list-group">
                                                        <li class="list-group-item"><b>Name : </b>{{ orders.name }}</li>
                                                        <li class="list-group-item"><b>Phone : </b>{{ orders.phone }}</li>
                                                        <li class="list-group-item"><b>Address : </b>{{ orders.address }}</li>
                                                        <li class="list-group-item"><b>Date : </b>{{ orders.order_date }}</li>
                                                        <li class="list-group-item"><b>Pay Through : </b>{{ orders.payby }}</li>
                                                    </ul>
                                                </div>
                                                <div class="card-footer"></div>
                                            </div>
                                        </div>
                                        <div class="col-lg-6 mb-4">
                                            <!-- Simple Tables -->
                                            <div class="card">
                                                <div class="card-header py-3 d-flex flex-row align-items-center justify-content-between">
                                                    <h6 class="m-0 font-weight-bold text-primary">Order Details</h6>
                                                </div>
                                                <div class="table-responsive">
                                                    <ul class="list-group">
                                                        <li class="list-group-item"><b>Sub Toatal : </b>{{ orders.subtotal }} tk</li>
                                                        <li class="list-group-item"><b>VAT : </b>{{ orders.vat }} %</li>
                                                        <li class="list-group-item"><b>Total : </b>{{ orders.total }} tk</li>
                                                        <li class="list-group-item"><b>Pay Amount : </b>{{ orders.pay }} tk</li>
                                                        <li class="list-group-item"><b>Due Amount : </b>{{ orders.due }} tk</li>
                                                    </ul>
                                                </div>
                                                <div class="card-footer"></div>
                                            </div>
                                        </div>
                                    </div>
                                    <div class="row">
                                        <div class="col-lg-12 mb-4">
                                            <!-- Simple Tables -->
                                            <div class="card">
                                                <div class="card-header py-3 d-flex flex-row align-items-center justify-content-between">
                                                    <h6 class="m-0 font-weight-bold text-primary">Order Details</h6>
                                                </div>
                                                <div class="table-responsive">
                                                    <table class="table align-items-center table-flush">
                                                        <thead class="thead-light">
                                                            <tr>
                                                                <th>Product Name</th>
                                                                <th>Product Code</th>
                                                                <th>Image</th>
                                                                <th>Quantity</th>
                                                                <th>Unit Price</th>
                                                                <th>Total</th>
                                                            </tr>
                                                        </thead>
                                                        <tbody>
                                                            <tr v-for="detail in details" :key="detail.id">
                                                                <td>{{ detail.product_name }}</td>
                                                                <td>{{ detail.product_code }}</td>
                                                                <td><img :src="'/'+detail.image" id="em_photo" alt=""></td>
                                                                <td>{{ detail.product_quantity }}</td>
                                                                <td>{{ detail.product_price }} tk</td>
                                                                <td>{{ detail.sub_total }} tk</td>
                                                            </tr>
                                                            
                                                        </tbody>
                                                    </table>
                                                </div>
                                                <div class="card-footer"></div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    created(){
        if(!User.loggedIn()){
            this.$router.push({name: '/'});
        }
    },
    data(){
        return {
            orders:{},
            details:{},
            errors:{},
        }
    },
    created(){
        let id = this.$route.params.id;
        axios.get('/api/order/details/' + id)
        .then( ({data}) => (this.orders = data) )
        .catch(console.log('error'))

         axios.get('/api/order/orderdetails/' + id)
        .then( ({data}) => (this.details = data) )
        .catch(console.log('error'))

    },
    methods:{
        
    }
    
}
</script>

<style scoped>
    #em_photo{
        height: 40px;
        width: 40px;
    }
</style>