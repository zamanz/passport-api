<template>
    <div class="hold-transition login-page">
        <div class="login-box">
            <div class="login-logo">
                <a href="/login">Login to access</a>
            </div>
            <!-- /.login-logo -->
            <div class="card">
                <div class="card-body login-card-body">
                    <p class="login-box-msg">Sign in to start your session</p>

                    <form @submit.prevent="login">
                        
                        <div class="input-group mb-3">
                            <input id="email" type="email" class="form-control" name="email" v-model="form.email" :class="{ 'is-invalid': form.errors.has('username') }" placeholder="Email Address" autocomplete="email" autofocus>
                            <div class="input-group-append">
                                <div class="input-group-text">
                                    <span class="fa fa-envelope"></span>
                                </div>
                            </div>
                            <has-error :form="form" field="username"></has-error>
                        </div>
                        <div class="input-group mb-3">
                            <input id="password" type="password" class="form-control" name="password" v-model="form.password" :class="{ 'is-invalid': form.errors.has('password') }" placeholder="Password" autocomplete="current-password">
                            <div class="input-group-append">
                                <div class="input-group-text">
                                    <span class="fa fa-unlock-alt"></span>
                                </div>
                            </div>
                            <has-error :form="form" field="password"></has-error>
                        </div>
                        <div class="row">
                            <div class="col-sm-7 offset-sm-1">
                                <div class="icheck-primary">
                                    <input class="form-check-input" type="checkbox" name="remember" v-model="form.remember" id="remember">
                                    <label for="remember">
                                        Remember Me
                                    </label>
                                </div>
                            </div>
                            <!-- /.col -->
                            <div class="col-sm-4">
                                <button type="submit" v-if="!isLoading" class="btn btn-primary btn-block">Sign In</button>
                                <button v-if="isLoading" type="button" class="btn btn-primary btn-block" disabled>Sign In...</button>
                            </div>
                            <!-- /.col -->
                        </div>
                    </form>
                    <p class="mb-1">
                        <a href="">I forgot my password</a>
                    </p>
                </div>
                <!-- /.login-card-body -->
            </div>
        </div>
        <!-- /.login-box -->
        
    </div>
</template>

<script>
    export default {
        data(){
            return {
                form: new Form({
                    email: null,
                    password: null,
                }),
                isLoading: false,
            }
        },
        mounted(){
            
        },
        computed:{

        },
        methods:{
            login(){
                this.isLoading = true;
                new Promise((result, reject) => {
                    this.form.post(baseURL + '/api/login').then((response) =>{
                        this.$store.commit('token', response.data.access_token);
                        localStorage.setItem('access_token', response.data.access_token);
                        this.$router.push({name:'home'});
                        this.isLoading = false;
                    }).catch((error) =>{
                        this.isLoading = false;
                        Swal.fire({
                            title: 'Error',
                            text: 'Email or Password Not Match :)',
                            icon: 'error'
                        })
                    })
                });
            },
        }
        
    }
</script>