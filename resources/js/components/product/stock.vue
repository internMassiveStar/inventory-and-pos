<template>
    <div>
        <router-link to="/store-product" class="btn btn-primary btn-sm">Add Product</router-link>
       <br/>
       <br/>
        <div class="row">
            <div class="col-lg-12 mb-4">
                <!-- Simple Tables -->
                <div class="card">
                    <div class="card-header py-3 d-flex flex-row align-items-center justify-content-between">
                        <h6 class="m-0 font-weight-bold text-primary">Stock</h6>
                        <input type="text" v-model="searchTerm" class="form-control col-md-4" placeholder="search here..">
                    </div>
                    <div class="table-responsive">
                        <table class="table align-items-center table-flush">
                            <thead class="thead-light">
                                <tr>
                                    <th>Product Name</th>
                                    <th>Product Code</th>
                                    <th>Category</th>
                                    <th>Buying Price</th>
                                    <th>Status</th>
                                    <th>Product Quantity</th>
                                    <th>Image</th>
                                    <th>Action</th>
                                </tr>
                            </thead>
                            <tbody>

                                <tr v-for="product in filterSearch" :key="product.id">
                                    <td>{{ product.product_name }}</td>
                                    <td>{{ product.product_code }}</td>
                                    <td>{{ product.category_name }}</td>
                                    <td>{{ product.buying_price }}</td>

                                    <td v-if="product.product_quantity >= 1"><span class="badge badge-success">Available</span></td>
                                    <td v-else><span class="badge badge-danger">Stock Out</span></td>

                                    <td>{{ product.product_quantity }}</td>
                                    <td><img :src="product.image" alt="" id="em_photo"></td>
                                    <td>
                                        <router-link :to="{name: 'edit-stock', params:{id:product.id}}" class="btn btn-primary btn-sm">Edit</router-link>
                                    </td>
                                </tr>
                               
                            </tbody>
                        </table>
                    </div>
                    <div class="card-footer"></div>
                </div>
            </div>
        </div>
        <!--Row-->
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
        return{
            products:[],
            searchTerm: '',
        }
    },

    computed:{
        filterSearch(){
            return this.products.filter(product => {
                return product.product_name.match(this.searchTerm);
            });
        }
    },
    
    methods:{
        allProduct(){
            axios.get('/api/product/')
            .then( ({data}) => (this.products = data))
            .catch()
        },
    },
    created(){
        this.allProduct();
    }
    
}
</script>

<style scoped>
    #em_photo{
        height: 40px;
        width: 40px;
    }
</style>