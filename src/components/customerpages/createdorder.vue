<template>
    <div>
            <div class="row justify-content-center">
                
                <div class="col-md-8">
                    <h3 class="mb-3 text-center">訂單資訊</h3>
                    <div class="row">
                        <div class="col-md-4">
                            <div class="text-center alert alert-success" role="alert">
                                訂單資料填寫
                            </div>
                        </div>
                        <div class="col-md-4">
                            <div class="text-center alert alert-dark"  role="alert">
                                付款
                            </div>
                        </div>
                        <div class="col-md-4">
                            <div class="text-center alert alert-dark"  role="alert">
                                完成訂單
                            </div>
                        </div>
                    </div>
                    <form class="" @submit.prevent="createOrder">
                        <div class="form-group">
                        <label for="useremail">Email</label>
                        <input type="email" :class="{'is-invalid':errors.first('email')}" class="form-control" name="email" id="useremail"
                            v-model="form.user.email" placeholder="請輸入 Email" v-validate="'required|email'">
                        <span class="text-danger">{{ errors.first('email') }}</span>
                        </div>
  
                        <div class="form-group">
                        <label for="username">收件人姓名</label>
                        <input :class="{'is-invalid':errors.has('name')}" type="text" class="form-control" name="name" id="username"
                            v-model="form.user.name" placeholder="輸入姓名" v-validate="'required'">
                        <span class="text-danger" v-if="errors.has('name')">請輸入姓名</span>
                        </div>
                    
                        <div class="form-group">
                        <label for="usertel">收件人電話</label>
                        <input :class="{'is-invalid':errors.has('tel')}" type="tel" class="form-control" name="tel" id="usertel" v-model="form.user.tel"
                            v-validate="'required'"
                            placeholder="請輸入電話">
                        <span  v-if="errors.has('tel')" class="text-danger">請輸入電話號碼</span>
                        </div>
                    
                        <div class="form-group">
                        <label for="useraddress">收件人地址</label>
                        <input type="text" :class="{'is-invalid':errors.has('address')}" class="form-control" name="address" id="useraddress" 
                            v-validate="'required'"
                            v-model="form.user.address"
                            placeholder="請輸入地址">
                        <span  v-if="errors.has('address')" class="text-danger">地址欄位不得留空</span>
                        </div>
                    
                        <div class="form-group">
                        <label for="comment">留言</label>
                        <textarea name="" id="comment" class="form-control" cols="30" rows="10" v-model="form.message"></textarea>
                        </div>
                        <div class="text-right">
                        <button class="btn btn-danger">送出訂單</button>
                        </div>
                    </form>
                </div>
            
            </div>
    </div>
</template>


<script>

    export default{
        data(){
            return {
                carts:[],
                form:{
                    user:{
                        name:'',
                        email:'',
                        tel:'',
                        address:'',
                    },
                    message:'',
                    }
            }
        },
        methods:{
                createOrder(){
                        const vm = this ;
                        vm.$store.dispatch('updateLoading',true);
                        const url = `${process.env.APIPATH}/api/${process.env.CUSTOMPATH}/order`;
                        this.$validator.validate().then((result)=>{
                            if(result){
                            vm.$http.post(url,{data:vm.form}).then((response)=>{
                            console.log("訂單已建立",response);
                            console.log(response.data);
                            if(response.data.success){
                                vm.$router.push(`/customer_website/checkout_page/checkout/${response.data.orderId}`);
                            }
                            vm.$bus.$emit('messsage:push',"訂單成立",'success');
                            vm.$store.dispatch('getCart');
                            vm.$store.dispatch('updateLoading',false);
                            });
                        }else{
                            console.log('請輸入完整');
                            vm.$store.dispatch('updateLoading',false);
                        }
                        
                    })
            }
        },
        created(){
        }
    }
</script>

<style lang="scss" scope>

</style>