<template>
    <div>
        <router-link to="/store-employee" class="btn btn-primary btn-sm">Add Employee</router-link>
       <br/>
       <br/>
        <div class="row">
            <div class="col-lg-12 mb-4">
                <!-- Simple Tables -->
                <div class="card">
                    <div class="card-header py-3 d-flex flex-row align-items-center justify-content-between">
                        <h6 class="m-0 font-weight-bold text-primary">Employee List</h6>
                        <input type="text" v-model="searchTerm" class="form-control col-md-4" placeholder="search by phone">
                    </div>
                    <div class="table-responsive">
                        <table class="table align-items-center table-flush">
                            <thead class="thead-light">
                                <tr>
                                    <th>Name</th>
                                    <th>Photo</th>
                                    <th>Phone</th>
                                    <th>Joining Date</th>
                                    <th>Salary</th>
                                    <th>NID</th>
                                    <th>Action</th>
                                </tr>
                            </thead>
                            <tbody>

                                <tr v-for="employee in filterSearch" :key="employee.id">
                                    <td>{{ employee.name }}</td>
                                    <td><img :src="employee.photo" alt="" id="em_photo"></td>
                                    <td>{{ employee.phone }}</td>
                                    <td>{{ employee.joining_date }}</td>
                                    <td>{{ employee.salary }}</td>
                                    <td>{{ employee.nid }}</td>
                                    <td>
                                        <router-link :to="{name: 'edit-employee', params:{id:employee.id}}" class="btn btn-primary btn-sm">Edit</router-link>
                                        <a @click="deleteEmployee(employee.id)" class="btn btn-sm btn-danger">Delete</a>
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
            employees:[],
            searchTerm: '',
        }
    },

    computed:{
        filterSearch(){
            return this.employees.filter(employee => {
                return employee.phone.match(this.searchTerm);
            });
        }
    },
    
    methods:{
        allEmployee(){
            axios.get('api/employee')
            .then( ({data}) => (this.employees = data))
            .catch()
        },
        deleteEmployee(id){
            Swal.fire({
            title: 'Are you sure?',
            text: "You won't be able to revert this!",
            icon: 'warning',
            showCancelButton: true,
            confirmButtonColor: '#3085d6',
            cancelButtonColor: '#d33',
            confirmButtonText: 'Yes, delete it!'
            }).then((result) => {
                if (result.isConfirmed) {

                    axios.delete('api/employee/' + id)
                    .then( () => {
                        this.employees = this.employees.filter(employee => {
                            return employee.id != id
                        })
                    } )
                    .catch( () => {
                        this.$router.push({name: 'employee'})
                    } )

                    Swal.fire(
                    'Deleted!',
                    'Your file has been deleted.',
                    'success'
                    )
                }
            })
        }
    },
    created(){
        this.allEmployee();
    }
    
}
</script>

<style scoped>
    #em_photo{
        height: 40px;
        width: 40px;
    }
</style>