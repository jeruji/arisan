<template>
    <div class="authentication-forms">
        <form class="form-signin" v-bind:style="[activeForm=='signIn' ? {'display': 'block'} : {'display': 'none'}]">
            <h1 class="h3 mb-3 font-weight-normal" style="text-align: center">Masuk untuk memulai arisan</h1>
            <div class="social-login">
                <button class="btn facebook-btn social-btn" type="button"><span><i class="fab fa-facebook-f"></i> Masuk Dengan Facebook</span> </button>
                <button @click="googleSignIn" class="btn google-btn social-btn" type="button"><span><i class="fab fa-google"></i> Masuk Dengan Google</span> </button>
            </div>
            <p style="text-align:center"> Atau  </p>
            <input type="email" id="inputEmail" v-model="userEmail" class="form-control" placeholder="Alamat Email" required="" autofocus="">
            <input type="password" id="inputPassword" v-model="userPassword" class="form-control" placeholder="Password" required="">
            
            <button @click.prevent="loginWithEmail" class="btn btn-success btn-block" type="submit"><i class="fas fa-sign-in-alt"></i> Masuk</button>
            <a href="#" @click.prevent="activeForm = 'forgotPassword'">Lupa Password?</a>
            <hr>
            <button class="btn btn-primary btn-block" type="button" @click="activeForm = 'register'"><i class="fas fa-user-plus"></i> Buat Akun</button>
        </form>

        <form action="/reset/password/" class="form-reset" v-bind:style="[activeForm=='forgotPassword' ? {'display': 'block'} : {'display': 'none'}]">
            <input type="email" id="resetEmail" class="form-control" placeholder="Alamat Email" required="" autofocus="">
            <button class="btn btn-primary btn-block" type="submit">Reset Password</button>
            <a href="#" @click.prevent="activeForm = 'signIn'"><i class="fas fa-angle-left"></i> Kembali</a>
        </form>

        <form action="/signup/" class="form-signup" v-bind:style="[activeForm=='register' ? {'display': 'block'} : {'display': 'none'}]">
            <div class="social-login">
                <button class="btn facebook-btn social-btn" type="button"><span><i class="fab fa-facebook-f"></i> Daftar Dengan Facebook</span> </button>
            </div>
            <div class="social-login">
                <button @click="googleSignIn" class="btn google-btn social-btn" type="button"><span><i class="fab fa-google"></i> Daftar Dengan Google</span> </button>
            </div>
            
            <p style="text-align:center">OR</p>

            <input type="text" id="user-name" v-model="fullname" class="form-control" placeholder="Nama Lengkap" required autofocus="">
            <span class="warning-text" v-bind:style="[fullnameCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Mohon isi nama lengkap anda</span>
            <input type="email" id="user-email" v-model="email" class="form-control" placeholder="Alamat Email" required autofocus="">
            <span class="warning-text" v-bind:style="[emailCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Mohon isi email anda</span>
            <input type="password" id="user-pass" v-model="password" class="form-control" placeholder="Password" required autofocus="">
            <span class="warning-text" v-bind:style="[passwordCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Password minimal 8 karakter dan terdapat minimal 1 angka</span>
            <input type="password" id="user-repeatpass" v-model="reEnterPassword" class="form-control" placeholder="Ulangi Password" required autofocus="">
            <span class="warning-text" v-bind:style="[rePasswordCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Password harus sama</span>

            <button class="btn btn-primary btn-block" @click.prevent="register" type="submit"><i class="fas fa-user-plus"></i> Daftar</button>
            <a href="#" @click.prevent="activeForm = 'signIn'"><i class="fas fa-angle-left"></i> Kembali</a>
        </form>
        <br>
    </div>
</template>
<script>
import firebase from 'firebase'
import Swal from 'sweetalert2'
import { server } from '@/static/helper'
import axios from 'axios'
export default {
    data() {
        return{
            activeForm: 'signIn',
            fullnameCondition: false,
            emailCondition: false,
            passwordCondition: false,
            rePasswordCondition: false,
            userEmail: '',
            userPassword: '',
            fullname: '',
            email: '',
            password: '',
            reEnterPassword: '',
            authenticatedUser: false
        }
    },
    created() {
        firebase.auth().onAuthStateChanged(function(user){
            if(user && user.emailVerified === false){
                user.sendEmailVerification().then(function(){
                    Swal.fire({
                        icon: 'info',
                        title: 'Registrasi Sukses',
                        text: 'Link verifikasi telah dikirim ke email anda '+
                                'Silahkan lakukan pengecekan.'  
                    }).then(function(){
                        window.location = '/'
                    })
                })
            }
        })
    },
    methods: {
        register() {
            this.validate()

            if(this.fullnameCondition==false&&this.emailCondition==false&&this.passwordCondition==false&&this.rePasswordCondition==false){
                let customerData = {  
                    fullname: this.fullname,
                    email: this.email
                }
                firebase.auth().createUserWithEmailAndPassword(this.email, this.password)
                .then(result=>{
                    customerData.firebaseId = firebase.auth().currentUser.uid
                    axios.post(server.baseURL+'/customer/create', customerData)
                })
                .catch(error=>{
                    Swal.fire({
                        icon: 'error',
                        title: 'Registrasi Gagal',
                        text: error.message  
                    })
                })
            }
        },
        validate(){
            let fullnameRegex = /^[a-zA-Z ]{2,30}$/
            let emailRegex = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
            let passwordRegex = /^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,}$/

            if(!fullnameRegex.test(this.fullname)){
                this.fullnameCondition = true
            }
            else{
                this.fullnameCondition = false
            }

            if(!emailRegex.test(this.email)){
                this.emailCondition = true
            }
            else{
                this.emailCondition = false
            }

            if(!passwordRegex.test(this.password)){
                this.passwordCondition = true
            }
            else{
                this.passwordCondition = false
            }

            if(this.reEnterPassword!=this.password){
                this.rePasswordCondition = true
            }
            else{
                this.rePasswordCondition = false
            }
        },
        googleSignIn(){
            const provider = new firebase.auth.GoogleAuthProvider()

            let customerData = {}

            firebase.auth().signInWithPopup(provider).then((result)=>{
                customerData.fullname = result.user.displayName
                customerData.email = result.user.email
                customerData.firebaseId = result.user.uid

                axios.post(server.baseURL+'/customer/create', customerData)
                .then(res=> {
                   Swal.fire({
                        icon: 'info',
                        title: 'Registrasi Sukses',
                        text: 'Terimakasih sudah melakukan registrasi, '+
                                'Anda akan dialihkan ke page sebelumnya'  
                    }).then(function(){
                        window.location = '/'
                    })
                })
            })
            .catch((error)=>{
                Swal.fire({
                        icon: 'error',
                        title: 'Registrasi Gagal',
                        text: error.message  
                    })
            })
        },
        loginWithEmail(){
            firebase.auth().signInWithEmailAndPassword(this.userEmail, this.userPassword)
            .then((result)=>{
                console.log(result)
            })
            .catch((error)=>{
                Swal.fire({
                        icon: 'error',
                        title: 'Masuk Gagal',
                        text: error.message  
                    })
            })
        },
        __submitToServer(data) {
            axios.post(server.baseURL+'/customer/create', data).then(() => {
                router.push('/login')
            })
        }
    }
}
</script>
<style scoped>
.authentication-forms{
    width:412px;
    margin:10vh auto;
    background-color:#f3f3f3;
    box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
    transition: all 0.3s cubic-bezier(.25,.8,.25,1);
}
.authentication-forms form {
    width: 100%;
    max-width: 410px;
    padding: 15px;
    margin: auto;
}
.authentication-forms .form-control {
    position: relative;
    box-sizing: border-box;
    height: auto;
    padding: 10px;
    font-size: 16px;
}
.authentication-forms .form-control:focus { z-index: 2; }
.authentication-forms .form-signin input[type="email"] {
    margin-bottom: -1px;
    border-bottom-right-radius: 0;
    border-bottom-left-radius: 0;
}
.authentication-forms .form-signin input[type="password"] {
    border-top-left-radius: 0;
    border-top-right-radius: 0;
}

.authentication-forms .social-login{
    width:390px;
    margin:0 auto;
    margin-bottom: 14px;
}
.authentication-forms .social-btn{
    font-weight: 100;
    color:white;
    width:190px;
    font-size: 0.9rem;
}

.authentication-forms a{
    display: block;
    padding-top:10px;
    color:lightseagreen;
}

.authentication-forms .lines{
    width:200px;
    border:1px solid red;
}

.warning-text{
    color: red;
    font-size: 12px;
    margin-top: 5px;
    margin-bottom: 5px;
}

.authentication-forms button[type="submit"]{ margin-top:10px; }

.authentication-forms .facebook-btn{  background-color:#3C589C; }

.authentication-forms .google-btn{ background-color: #DF4B3B; }

.authentication-forms .form-signup .social-btn{ width:210px; }

.authentication-forms .form-signup input { margin-bottom: 2px;}

.form-signup .social-login{
    width:210px !important;
    margin: 0 auto;
    margin-bottom: 14px;
}

@media screen and (max-width:500px){
    .authentication-forms{
        width:300px;
    }
    
    .authentication-forms  .social-login{
        width:200px;
        margin:0 auto;
        margin-bottom: 10px;
    }
    .authentication-forms  .social-btn{
        font-size: 1.3rem;
        font-weight: 100;
        color:white;
        width:200px;
        height: 56px;
        
    }
    .authentication-forms .social-btn:nth-child(1){
        margin-bottom: 5px;
    }
    .authentication-forms .social-btn span{
        display: none;
    }
    .authentication-forms  .facebook-btn:after{
        content:'Facebook';
    }
  
    .authentication-forms  .google-btn:after{
        content:'Google+';
    }
}
</style>