<template>
   <div>
       <div class="row justify-content-center">
            <div class="col-xl-6 col-lg-6 col-md-6">
                <div class="card shadow-sm my-5">
                    <div class="card-body p-0">
                        <div class="row">
                            <div class="col-lg-8 mx-auto">
                                <div class="login-form">
                                    <div class="text-center">
                                        <h1 class="h4 text-gray-900 mb-4">Login</h1>
                                    </div>
                                    <form class="user" @submit.prevent="login">
                                        <div class="form-group">
                                            <input type="email" class="form-control" v-model="form.email" placeholder="Enter Email Address" />
                                            <small class="text-danger" v-if="errors.email">{{ errors.email[0] }}</small>
                                        </div>
                                        <div class="form-group">
                                            <input type="password" class="form-control" v-model="form.password" placeholder="Password" />
                                            <small class="text-danger" v-if="errors.password">{{ errors.password[0] }}</small>
                                        </div>
                                        <div class="form-group">
                                            <div class="custom-control custom-checkbox small" style="line-height: 1.5rem;">
                                                <input type="checkbox" class="custom-control-input" id="customCheck" />
                                                <label class="custom-control-label" for="customCheck">Remember Me</label>
                                            </div>
                                        </div>
                                        <div class="form-group">
                                            <button type="submit" class="btn btn-primary btn-block">Login</button>
                                        </div>
                                    </form>
                                    <hr />
                                    <div class="text-center">
                                         <router-link to="/register">Create an Account!</router-link>
                                    </div>
                                    <div class="text-center">
                                         <router-link to="/forget">Forget Password?</router-link>
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
        if(User.loggedIn()){
            this.$router.push({name: 'home'});
        }
    },
    data(){
        return {
            form: {
                email: null,
                password: null
            },
            errors:{}
        }
    },
    methods: {
        login(){
            axios.post('/api/login', this.form)
            .then(response => {
                User.responseAfterLogin(response)
                Toast.fire({
                    icon: 'success',
                    title: 'Signed in successfully'
                })
                this.$router.push({name: 'home'})
            })
            .catch(error => this.errors = error.response.data.errors)
            .catch(
                Toast.fire({
                    icon: 'warning',
                    title: 'Invalid Email or Password'
                })
            )
        }
    }
}
</script>

<style scoped>

</style>