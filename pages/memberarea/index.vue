<template>
        <form class="form-container" v-bind:style="[userProfile==false ? {'display':'block'} : {'display':'none'}]">
            <div class="text-center">
                <h1>Anda harus melengkapi data sebelum dapat melanjutkan</h1>
            </div>
            <div class="margin-top-med">&nbsp;</div>
            <div class="row justify-content-center">
                <div class="form-group col-md-5">
                    <label class="grey-text" for="user-address">Alamat tempat tinggal</label>
                    <input type="text" name="user-address" v-model="userAddress" class="form-control" required="" autofocus=""/>
                    <span class="warning-text" v-bind:style="[addressCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Mohon isi alamat anda</span>
                </div>

                <div class="form-group col-md-5">
                    <label class="grey-text" for="user-phone">Nomor telepon</label>
                    <input type="text" name="user-phone" v-model="userPhone" class="form-control" required="" autofocus=""/>
                    <span class="warning-text" v-bind:style="[phoneCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Mohon isi nomor telepon anda</span>
                </div>
            </div>
            
            <div class="row justify-content-center">
                <div class="form-group col-md-5">
                    <label class="grey-text" for="user-dob">Tanggal lahir</label>
                    <input id="userDobID" name="user-dob" type="text" v-model="userDob" class="form-control" required="" autofocus="" readonly/>
                    <span class="warning-text" v-bind:style="[dobCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Mohon isi tanggal lahir anda</span>
                </div>

                <div class="form-group col-md-5">
                    <label class="grey-text" for="user-occupation">Pekerjaan</label>
                    <input type="text" name="user-occupation" v-model="userOccupation" class="form-control" required="" autofocus=""/>
                    <span class="warning-text" v-bind:style="[occupationCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Mohon isi pekerjaan anda</span>
                </div>
            </div>

            <div class="row justify-content-center">
                <div class="form-group col-md-5">
                    <label class="grey-text" for="user-work-address">Alamat kantor</label>
                    <input type="text" name="user-work-address" v-model="userWorkAddress" class="form-control" required="" autofocus=""/>
                    <span class="warning-text" v-bind:style="[workAddressCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Mohon isi alamat kantor anda</span>
                </div>
                
                <div class="form-group col-md-5">
                    <label class="grey-text" for="user-work-phone">Nomor telepon kantor</label>
                    <input type="text" name="user-work-phone" v-model="userWorkPhone" class="form-control" required="" autofocus=""/>
                    <span class="warning-text" v-bind:style="[workPhoneCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Mohon isi nomor telepon kantor anda</span>
                </div>
            </div>    
            
            <div class="row justify-content-center">
            <div class="form-group col-md-5">
                <label class="grey-text" for="user-status">Status</label>
                <select name="user-status" v-model="userStatus" class="form-control">
                    <option value="-- Pilih status --">-- Pilih status --</option>
                    <option value="lajang">Lajang</option>
                    <option value="menikah">Menikah</option>
                    <option value="bercerai">Bercerai</option>
                </select>
                <span class="warning-text" v-bind:style="[statusCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Mohon pilih status anda</span>
            </div>    

                <div class="form-group col-md-5">
                    <label for="user-salary">Gaji</label>
                    <input type="text" name="user-salary" v-model="userSalary" class="form-control" required="" autofocus=""/>
                    <span class="warning-text" v-bind:style="[salaryCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Mohon isi gaji anda</span>
                </div>
            </div>

            <div class="row justify-content-center">
                <div class="form-group col-md-5">
                    <label for="sib-full-name">Nama saudara tidak serumah</label>
                    <input type="text" v-model="sibFullname" class="form-control" required="" autofocus=""/>
                    <span class="warning-text" v-bind:style="[sibFullnameCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Mohon isi nama saudara tidak serumah</span>
                </div>
                
                <div class="form-group col-md-5">
                    <label for="sib-address">Alamat saudara tidak serumah</label>
                    <input type="text" name="sib-address" v-model="sibAddress" class="form-control" required="" autofocus=""/>
                    <span class="warning-text" v-bind:style="[sibAddressCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Mohon isi alamat saudara tidak serumah</span>
                </div>
            </div>

            <div class="row justify-content-center">
                <div class="form-group col-md-5">
                    <label for="sib-phone">Nomor telepon saudara tidak serumah</label>
                    <input type="text" name="sib-phone" v-model="sibPhone" class="form-control" required="" autofocus=""/>
                    <span class="warning-text" v-bind:style="[sibPhoneCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Mohon isi nomor telepon saudara tidak serumah</span>
                </div>
                
                <div class="form-group col-md-5">
                    <label for="sib-dob">Tanggal lahir saudara tidak serumah</label>
                    <input type="text" name="sib-dob" v-model="sibDob" class="form-control" required="" autofocus=""/>
                    <span class="warning-text" v-bind:style="[sibDobCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Mohon isi tanggal lahir saudara tidak serumah</span>
                </div>
            </div>
            
            <div class="row justify-content-center">
                <div class="form-group col-md-5">
                    <label for="ktp-file" class="custom-file-upload">Upload KTP anda</label>
                    <span class="btn btn-primary btn-file">
                        Pilih File KTP
                        <input type="file" name="ktp-file" ref="ktpImgFile" v-on:change="handleFileUpload('ktp')">
                    </span>
                    <span class="warning-text" v-bind:style="[ktpCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Mohon upload KTP anda</span>
                </div>
                
                <div class="form-group col-md-5">
                    <label for="kk-file" class="custom-file-upload">Upload KK anda</label>
                    <span class="btn btn-primary btn-file">
                        Pilih File KK
                        <input type="file" name="kk-file" ref="kkImgFile" v-on:change="handleFileUpload('kk')">
                    </span>    
                    <span class="warning-text" v-bind:style="[kkCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Mohon upload KK anda</span>
                </div>
            </div>

            <div class="row justify-content-center">
                <div class="form-group col-md-5">
                    <label for="rekening-file" class="custom-file-upload">Upload halaman 1 buku rekening anda</label>
                    <span class="btn btn-primary btn-file">
                        Pilih File Halaman 1 Buku Rekening   
                        <input type="file" name="rekening-file" ref="rekeningImgFile" v-on:change="handleFileUpload('rekening')">
                    </span>
                    <span class="warning-text" v-bind:style="[rekeningCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Mohon upload halaman 1 buku rekening anda</span>
                </div>

                <div class="form-group col-md-5">
                    <label for="koran-file" class="custom-file-upload">Upload rekening koran anda</label>
                    <span class="btn btn-primary btn-file">
                        Pilih File Rekening Koran
                        <input type="file" name="koran-file" ref="koranImgFile" v-on:change="handleFileUpload('koran')">
                    </span>
                    <span class="warning-text" v-bind:style="[rekeningKoranCondition==true ? {'display': 'block'} : {'display': 'none'}]">*) Mohon upload rekening koran anda</span>
                </div>
            </div>

            <div class="row justify-content-center margin-top-med margin-bottom-med">
                <div class="form-group col-md-5">
                    <button class="btn btn-primary" @click.prevent="completeProfile" type="submit"><i class="fas fa-user-plus"></i> Submit</button>
                </div>
                <div class="form-group col-md-5">&nbsp;</div>
            </div>
        </form>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css?family=Trocchi&display=swap');
h1 { 
    color: #7c795d; 
    font-family: 'Trocchi', serif; 
    font-size: 30px; 
    font-weight: normal; 
    line-height: 48px; 
    margin: 0; 
}
.margin-top-med {
    margin-top: 40px;
}
.margin-bottom-med {
    margin-bottom: 40px;
}
.form-container {
    background-color: #f3f3f3;
    box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
    -webkit-transition: all 0.3s cubic-bezier(.25,.8,.25,1);
    transition: all 0.3s cubic-bezier(.25,.8,.25,1);
    width: 80%;
    margin-left: auto;
    margin-right: auto;
    margin-top: 50px;
}
.btn-file {
    position: relative;
    overflow: hidden;
    display: block;
    background-color: #DF4B3B;
    border-color: #DF4B3B;
}
.btn-file input[type="file"] {
    position: absolute;
    top: 0;
    right: 0;
    min-width: 100%;
    min-height: 100%;
    font-size: 100px;
    text-align: right;
    filter: alpha(opacity=0);
    opacity: 0;
    outline: none;
    background: white;
    cursor: inherit;
    display: block;
}
@media screen and (max-width: 786px) {
    h1{
        font-size: 20px;
        line-height: 30px;        
    }
    .form-container {
        width: 90%;
        padding: 20px;
    }
    .margin-bottom-med {
        margin-bottom: 0;
    }
}
</style>

<script>
import axios from 'axios'
import Swal from 'sweetalert2'
import { server } from '../../static/helper'

export default {
    data(){
        return{
            userProfile: false,
            userAddress: '',
            userPhone: '',
            userDob: '',
            userOccupation: '',
            userWorkAddress: '',
            userWorkPhone: '',
            userStatus: '-- Pilih status --',
            userSalary: '',
            sibFullname: '',
            sibAddress: '',
            sibPhone: '',
            sibDob: '',
            ktpFile: null,
            kkFile: null,
            rekeningFile: null,
            koranFile: null,
            addressCondition: false,
            phoneCondition: false,
            dobCondition: false,
            occupationCondition: false,
            workAddressCondition: false,
            workPhoneCondition: false,
            statusCondition: false,
            salaryCondition: false,
            sibFullnameCondition: false,
            sibAddressCondition: false,
            sibPhoneCondition: false,
            sibDobCondition: false,
            ktpCondition: false,
            kkCondition: false,
            rekeningCondition: false,
            rekeningKoranCondition: false
        }
    },
    methods: {
        handleFileUpload(type){
            const self = this

            if(type=='ktp'){
                let ktpFile = this.$refs.ktpImgFile.files[0]
                let formData = new FormData()
                formData.append('file', ktpFile)

                axios.post(server.baseURL+'/customer/upload', formData,
                    {
                        headers: {
                            'Content-Type': 'multipart/form-data'
                        }
                    }
                ).then(response => {
                    self.ktpFile = response.data.filename
                    Swal.fire({
                        icon: 'info',
                        title: 'Upload KTP Sukses',
                        text: 'Image KTP anda sudah masuk ke server kami'  
                    })
                })
                .catch(response => {
                    Swal.fire({
                        icon: 'error',
                        title: 'Upload KTP Gagal',
                        text: 'Upload gagal, mohon periksa format file anda'  
                    })
                })
            }
            else if(type=='kk'){
                let kkFile = this.$refs.kkImgFile.files[0]
                let formData = new FormData()
                formData.append('file', kkFile)

                axios.post(server.baseURL+'/customer/upload', formData,
                    {
                        headers: {
                            'Content-Type': 'multipart/form-data'
                        }
                    }
                ).then(response => {
                    self.kkFile = response.data.filename
                    Swal.fire({
                        icon: 'info',
                        title: 'Upload KK Sukses',
                        text: 'Image KK anda sudah masuk ke server kami'  
                    })
                })
                .catch(response => {
                    Swal.fire({
                        icon: 'error',
                        title: 'Upload KK Gagal',
                        text: 'Upload gagal, mohon periksa format file anda'  
                    })
                })
            }
            else if(type=='rekening'){
                let rekeningFile = this.$refs.rekeningImgFile.files[0]
                let formData = new FormData()
                formData.append('file', rekeningFile)

                axios.post(server.baseURL+'/customer/upload', formData,
                    {
                        headers: {
                            'Content-Type': 'multipart/form-data'
                        }
                    }
                ).then(response => {
                    self.rekeningFile = response.data.filename
                    Swal.fire({
                        icon: 'info',
                        title: 'Upload Buku Tabungan Sukses',
                        text: 'Image halaman satu buku tabungan anda sudah masuk ke server kami'  
                    })
                })
                .catch(response => {
                    Swal.fire({
                        icon: 'error',
                        title: 'Upload Buku Tabungan Gagal',
                        text: 'Upload gagal, mohon periksa format file anda'  
                    })
                })
            }
            else if(type=='koran'){
                let koranFile = this.$refs.koranImgFile.files[0]
                let formData = new FormData()
                formData.append('file', koranFile)

                axios.post(server.baseURL+'/customer/upload', formData,
                    {
                        headers: {
                            'Content-Type': 'multipart/form-data'
                        }
                    }
                ).then(response => {
                    self.koranFile = response.data.filename
                    Swal.fire({
                        icon: 'info',
                        title: 'Upload Rekening Koran Sukses',
                        text: 'Image rekening koran anda sudah masuk ke server kami'  
                    })
                })
                .catch(response => {
                    Swal.fire({
                        icon: 'error',
                        title: 'Upload Rekening Koran Gagal',
                        text: 'Upload gagal, mohon periksa format file anda'  
                    })
                })
            }
        },
        completeProfile(){

            let submitProfile = true

            if(/^[a-zA-Z0-9\s,.'-]{3,}$/.test(this.userAddress)==false){
                this.addressCondition = true
                submitProfile = false
            }

            if(/^[\+]?[(]?[0-9]{3}[)]?[-\s\.]?[0-9]{3}[-\s\.]?[0-9]{4,6}$/.test(this.userPhone)==false){
                this.phoneCondition = true
                submitProfile = false
            }
                
            if(this.userDob==''){
                this.dobCondition = true
                submitProfile = false
            }

            if(this.userOccupation.length>=3){
                this.occupationCondition = true
                submitProfile = false
            }
                
            if(/^[a-zA-Z0-9\s,.'-]{3,}$/.test(this.userWorkAddress)==false){
                this.workAddressCondition = true
                submitProfile = false
            }
                
            if(/^[\+]?[(]?[0-9]{3}[)]?[-\s\.]?[0-9]{3}[-\s\.]?[0-9]{4,6}$/.test(this.userWorkPhone)==false){
                this.workPhoneCondition = true
                submitProfile = false
            }
                
            if(this.userStatus=='-- Pilih status --'){
                this.statusCondition = true
                submitProfile = false
            }
                
            if(/^[0-9]+$/.test(this.userSalary)==false){
                this.salaryCondition = true    
                submitProfile = false
            }

            if(/^\s*([A-Za-z]{1,}([\.,] |[-']| ))+[A-Za-z]+\.?\s*$/.test(this.sibFullname)==false){
                this.sibFullnameCondition = true
                submitProfile = false
            }
                
            if(/^[a-zA-Z0-9\s,.'-]{3,}$/.test(this.sibAddress)==false){
                this.sibAddressCondition = true
                submitProfile = false
            }
                
            if(/^[\+]?[(]?[0-9]{3}[)]?[-\s\.]?[0-9]{3}[-\s\.]?[0-9]{4,6}$/.test(this.sibPhone)==false){
                this.sibPhoneCondition = true
                submitProfile = false
            }
                
            if(this.sibDob==''){
                this.sibDobCondition = true
                submitProfile = false   
            }

            if(this.ktpFile==null){
                this.ktpCondition = true
                submitProfile = false
            }

            if(this.kkFile==null){
                this.kkCondition = true
                submitProfile = false
            }

            if(this.rekeningFile==null){
                this.rekeningCondition = true
                submitProfile = false
            }
                
            if(this.koranFile==null){
                this.rekeningKoranCondition = true
                submitProfile = false
            }
            
            if(submitProfile){
                let customerData = {  
                    address: this.userAddress,
                    phone: this.phone,
                    dob: this.userDob,
                    occupation: this.userOccupation,
                    workAddress: this.userWorkAddress,
                    workPhone: this.userWorkPhone,
                    status: this.userStatus,
                    salary: this.userSalary,
                    sibFullname: this.sibFullname,
                    sibAddress: this.sibAddress,
                    sibPhone: this.sibPhone,
                    sibDob: this.sibDob,
                    ktpFile: this.ktpFile,
                    kkFile: this.kkFile,
                    bukuRekeningFile: this.bukuRekeningFile,
                    rekeningKoranFile: this.rekeningKoranFile
                }
                this.submitProfile(customerData)
            }
        },
        submitProfile(data){

        }
    },
    created() {
        var self = this
    }
}
</script>