<template>
    <div>
        <div class="form">
            <h3>Đăng nhập</h3>
            <div>
                <label>Tài khoản</label>
                <input type="text" v-model="tk"><br>
            </div>
            <div>
                <label>Mật khẩu </label>
                <input type="password" v-model="mk"><br>
            </div>
            <button @click="login()">
                <i class="ti-arrow-circle-right"></i> 
                Đăng nhập
            </button>
            <h4 :class="[{success : loginSuccess==true},{fail : loginSuccess==false}]" v-if="(isLogin==true)">{{ resultLogin }}</h4>
        </div>
    </div>
</template>
<script>
    export default {
        name :'LoginForm',
        data(){
            return {
                isLogin : false,
                loginSuccess : false,
                tk : '',
                mk : '',
                resultLogin : 'Undefined',
                selfUser: {
                    name : '',
                    avatar : '',
                    check : true,
                    username : '',
                    subscribers : '',
                    subscribe : '',
                    videos : '',
                    banner : ''
                }
            }
        },
        props : {
            apiRoot : String,
            Connected : Boolean,
            user : {
                name : String,
                avatar : String,
                check : Boolean,
                username : String,
                subscribers : String,
                subscribe : Array,
                videos : Array,
                banner : String
            }
        },
        methods :{
            login : function(){
                var self = this
                fetch(this.apiRoot+'users')
                    .then((response)=>{
                        return response.json()
                    })
                    .then((data)=>{
                        var loginSuccess = false
                        data.forEach((account)=>{
                            if(account.taikhoan == this.tk && account.matkhau == this.mk){
                                loginSuccess = true
                                self.selfUser = Object.assign({},account)
                                this.update()
                            }
                        })
                        this.loginSuccess = loginSuccess
                        this.isLogin = true
                        this.resultLogin = this.loginSuccess == true ? 'Login thành công' : 'Login thất bại'
                    })
            },
            update : function(){
                this.$emit('update:user',this.selfUser)
                this.$emit('update:connected',true)
            }
        },
        watch: {
            selfUser : function(){
                // console
            }
        }
    }
</script>
<style scoped>

@keyframes TopToMiddle{
    from {
        margin-top : 0%;
        opacity: 0;
    }
    to{
        margin-top : 10%;
        opacity: 1;
    }
}

.form{
    background-color: #f0f3f8;
    margin-top : 10%;
    width: 600px;
    height: auto;
    min-height: 350px;
    border-radius: 10px;
    display: inline-block;
    animation: TopToMiddle ease 0.3s;
}

.form > h3{
    margin-top: 10%;
    margin-bottom: 5%;
}

.form input{
    background-color: rgba(70, 90, 126, 0.4);
    height: 30px;
    width: 250px;
    margin-left: 2%;
    padding : 0 6px;
    border-radius: 3px;
    font-size:16px ;
    margin-top : 2%;

}

button{
    margin-top : 15px;
    width : 250px;
    height: 50px;
    font-size: 16px;
    background-color: rgb(6,101,208);
    border : none;
    color : #fff;
    border-radius: 5px;
}

button:hover{
    cursor: pointer;
    opacity: 0.6;
}

i{
    font-size: 16px;
}

@keyframes bottomToMid{
    from {
        margin-top : 60px;
        opacity: 0;
    }
    to {
        margin-top : 40px;
        opacity: 1;
    }
}

h4{
    margin-top : 40px;
    animation: bottomToMid ease 0.3s;
}
.success{
    color : green;
}
.fail{
    color : red;
}
</style>