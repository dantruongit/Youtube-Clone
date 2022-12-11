<template>
    <div class="form">
        <h3>Tải lên</h3>
        <div>
            <label>Tên&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</label>
            <input type="text" v-model="video.name"><br>
        </div>
        <div>
            <label>Ảnh&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</label>
            <input type="text" v-model="video.image"><br>
        </div>
        <div>
            <label>iFrame</label>
            <input type="text" v-model="video.iframe"><br>
        </div>
        <div>
            <label>Gif&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</label>
            <input type="text" v-model="video.gif"><br>
        </div>
        <div>
            <label>Mô tả&nbsp;&nbsp;&nbsp;</label>
            <input type="text" v-model="video.description"><br>
        </div>
        <button @click="upload()">
            <i class="ti-arrow-circle-up"></i> 
            Tải lên
        </button>
        <h4 :class="[{success : uploadSuccess==true},{fail : uploadSuccess==false}]" v-if="(isUploaded==true)">{{ resultUpload }}</h4>
    </div>
</template>
<script>
export default {
    name : 'UploadVideo',
    data(){
        return {
            resultUpload :'',
            isUploaded : false, 
            uploadSuccess : false,
            video : {
                name : '',
                image : '',
                iframe : '',
                gif : '',
                view : '',
                time : '',
                tag : ["All"],
                author : '',
                description : ''
            }
        }
    },
    props:{
        user : Object,
        apiRoot : String,
        isreload : String
    },
    methods : {
        upload : function(){
            this.isUploaded = true
            this.video.view = Math.ceil(Math.random()*500 + 500) +'K views'
            this.video.time = Math.ceil(Math.random()*9 + 2) + ' months'
            fetch(this.apiRoot + 'videos',{
                method : 'POST',
                headers : {
                    'Content-Type' : 'application/json'
                },
                body : JSON.stringify(this.video)
            })
                .then((response)=>{
                    return response.json()
                })
                .then((result)=>{
                    this.uploadSuccess = true
                    this.resultUpload = 'Upload thành công'
                    console.log(result)
                })
                .catch(()=>{
                    this.resultUpload = 'Upload thất bại'
                    this.uploadSuccess = false
                })
            this.$emit('update:isreload',Math.random().toFixed(5)+'')
        }
    },
    created : function(){
        this.video.author = this.user.id
    }
}
</script>
<style scoped>

@keyframes TopToMiddle{
    from {
        margin-top : -10%;
        opacity: 0;
    }
    to{
        margin-top : 3%;
        opacity: 1;
    }
}

.form{
    background-color: #f0f3f8;
    margin-top : 3%;
    width: 600px;
    height: auto;
    min-height: 500px;
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