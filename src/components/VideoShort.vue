<template>
    <div class="video-short">
        <div class="video">
            <video :src="link_short" controls></video>
            <img class="avatar" :src="link_avatar" alt="" width="32px" height="32px">
            <span class="author">{{ author }}</span>
            <div class="action">
                <div class="like" :class="[{clicked : liked==true}]" @click="change('like')">
                    <i class="ti-thumb-up"></i>
                    <p>{{ like }}</p>
                </div>
                <div class="dislike" :class="[{clicked : disliked==true}]" @click="change('dislike')">
                    <i class="ti-thumb-down"></i>
                    <p>Dislike</p>
                </div>
                <div class="comment">
                    <i class="ti-comment-alt"></i>
                    <p>{{ comment }}</p>
                </div>
                <div class="share">
                    <i class="ti-share"></i>
                    <p>Share</p>
                </div>
                <div class="menu">
                    <i class="ti-menu"></i>
                </div>
            </div>
            
        </div>
    </div>
</template>
<script>
    export default{
        name :'VideoShort',
        props :{
            avatar : String,
            author : String,
            source : String,
            like : String,
            comment : String,
            channel : {
                avatar : String,
                author : String,
                check : Boolean,
                username : String,
                subscribers : String,
                videos : Array
            }
        },
        components:{

        },
        data(){
            return{
                liked : false,
                disliked : false
            }
        },
        computed : {
            link_short : function(){
                return '/assets/img/short/'+this.source
            },
            link_avatar : function(){
                return '/assets/img/avatar/' + this.avatar
            }
        },
        methods : {
            change : function(value){
                if(value == 'like'){
                    this.liked = !this.liked
                    this.disliked = false
                }
                else {
                    this.disliked = !this.disliked
                    this.liked = false
                }
            }
        },
    }
</script>
<style scoped>

.clicked{
    background-color: #fff !important;
    color : #000 !important;
}

.video-short{
    position: relative;
    margin: 25px 0 150px 0;
    width: 100%;
    height: 500px;
}

.video{
    height: 100%;
    position: absolute;
    left : 50%;
    transform: translateX(-50%);
    width: 500px;
}


.video video{
    /* 280 x 498 */
    width : auto;
    border-radius: 10px;
    height : 100%;
    max-width: 290px;
}

.video .avatar{
    position: absolute;
    border-radius: 50%;
    left : 25%;
    bottom : 15%;
    cursor: pointer;
}

.video .author{
    position: absolute;
    left : 35%;
    bottom : 16%;
    color : #fff;
    font-family: Arial, Helvetica, sans-serif;
    user-select: none;
}


.action{
    height: 100%;
    position: absolute;
    width: 60px;
    top : 0;
    right : 6%;
}

.action::before{
    content: '';
    display: block;
    margin-top : 120%;
}

.action div{
    margin-bottom : 40px;
    --size : 48px;
    color : white;
    font-size: 15px;
    line-height: 25px;
    width: var(--size);
    height: var(--size);
    background-color: #717171;
    border-radius: 50%;
}

.action div:hover{
    background-color: #ccc;
    opacity: 0.7;
    cursor: pointer;
}


.action div i{
    line-height: var(--size);
    font-size: 20px;
    width: var(--size);
    height: var(--size);
}
.action div p{
    color : #fff;
}
</style>