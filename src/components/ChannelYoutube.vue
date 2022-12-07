<template>
    <div>
        <img class="banner" :src="link_banner(channel.banner)" alt="">
        <div class="channel">
            <div class="info">
                <div class="block-avatar">
                    <img class="avatar" :src="link_avatar(channel.avatar)" alt="">
                </div>
                <div class="info-channel left">
                    <h2 class="name left white">{{channel.author}}</h2> 
                    <div class="check"  v-if="channel.check">
                                <i class="ti-check"></i>
                    </div> <br>
                    <span class="left white blur-text">@{{channel.username}}</span> <br> <br>
                    <span class="left white blur-text">{{channel.subscribers}} subscribers</span>
                </div>
                <div class="subscribe">
                    <button class="btn-subscribe" :class="[{subscribed : sub=='Subscribed'}]" v-if="(currentUser != channel.author)" @click="subscribe()">{{sub}}</button>
                </div>
            </div>
        </div>
        <div class="header">
            <span :class="[{'selected' : (select == 0)}]" @click="(select = 0)">Home</span>
            <span :class="[{'selected' : (select == 1)}]" @click="(select = 1)">Videos</span>
            <span :class="[{'selected' : (select == 2)}]" @click="(select = 2)">Playlists</span>
            <span :class="[{'selected' : (select == 3)}]" @click="(select = 3)">Community</span>
            <span :class="[{'selected' : (select == 4)}]" @click="(select = 4)">Channels</span>
        </div>
        <hr>
        <div class="content">
            <div class="video" v-for="item of channel.videos" :key="item" @click="playVideo(channel.id,dataVideo[item].name)">
                <img class="image-video left" :src="link_image(dataVideo[item].image)" alt="">
                <p class="left white">{{ compact_name(dataVideo[item].name) }}</p> <br> <br>
                <span class="left blur-text">{{ channel.author }}</span>

                <div class="check"  v-if="channel.check">
                    <i class="ti-check"></i>
                </div>
                <span class="view left blur-text">{{dataVideo[item].view}} views • {{dataVideo[item].time}} ago </span> <br> <br>
                <span class="description left blur-text">{{ description(dataVideo[item].description) }}</span>
            </div>
        </div>
    </div>
</template>
<script>
    export default{
        name : 'ChannelYoutube',
        data(){
            return {
                select : 0,
                link_img : '',
                selfPlaying : {
                    author : '',
                    video : ''
                },
                sub : 'Subscribe',
                selfUser : {}
            }
        },
        props:{
            currentUser : String,
            channel : Object,
            playing : {
                author : String,
                video : String
            },
            user : Object,
            list_users : Array,
            dataVideo : Array,
            apiRoot : String
        },
        created : function(){
            this.selfUser = Object.assign({},this.user)
            if(this.selfUser.subscribe.includes(this.channel.id)){
                this.sub = 'Subscribed'
            }
        },
        methods : {
            subscribe : function(){
                this.sub = this.sub == 'Subscribe'?'Subscribed' : 'Subscribe';
                if(this.sub == 'Subscribed'){
                    if(this.selfUser.subscribe.indexOf(this.channel.id) == -1) 
                        this.selfUser.subscribe.push(this.channel.id)
                }
                else {
                    if(this.selfUser.subscribe.indexOf(this.channel.id) != -1){
                        this.selfUser.subscribe.splice(this.selfUser.subscribe.indexOf(this.channel.id),1)
                    }
                }
                this.$emit('update:user',this.selfUser)
                fetch(this.apiRoot+'users/'+this.selfUser.id,{
                    method : 'PUT',
                    headers:{
                        'Content-type' : 'application/json'
                    },
                    body : JSON.stringify(this.selfUser)
                })
            },
            compact_name : function(value){
                if(value.length >=50 )
                return value.substring(0,47) + '...' 
                return value
            },
            link_image : function(value){
                return '/assets/img/video/' + value
            },
            link_avatar : function(value){
                return '/assets/img/avatar/' + value
            },
            description : function(value){
                if(value!==undefined) {
                    if(value.length > 120)
                    return value.substring(0,120) + '...'
                    return value
                }
                return ''
            },
            link_banner : function(value){
                return '/assets/img/channel/' + value
            },
            dishover : function(item){
                this.link_img = this.link_image(item.image)
            },
            hover : function(item){
                this.link_img = this.link_image(item.gif)
            },
            playVideo : function(Author,videoName){
                var playnow = {
                    author:  Author,
                    video : videoName
                }
                this.selfPlaying = playnow
                this.$emit('update:playing',this.selfPlaying)
            }
        },
    }
</script>
<style scoped>

hr{
    border: 0.1px solid #333;
}
.left{
    float : left;
}

.white{
    color : #fff;
    line-height: 1.4rem;
}

.blur-text{
    opacity: 0.6;
    font-size: 15px;
    color : rgb(149, 135, 135);
}

.banner{
    width: 100%;
    max-height: 200px;
}

.channel, .header, .content{
    margin-top : 16px;
    padding : 0 16%;
}

.info{
    position: relative;
}

div.info::after{
    content: '';
    display: block;
    clear : both;
}

.info .info-channel{
    margin-left: 20px;
}
.info .block-avatar{
    --size : 100px;
    width: var(--size);
    height: var(--size);
    float : left;
}

.info .avatar{
    --size : 80px;
    width: var(--size);
    height: var(--size);
    border-radius: 50%;
}

i.ti-check{
    font-size: 12px;
}   

div.check{
    --size : 18px;
    opacity: 1;
    width : var(--size);
    height : var(--size);
    background-color: rgb(149, 135, 135) !important;
    border-radius: 50%;
    color : white;
    float : left;
    margin-left : 7px;
    margin-top : 4px;
}

.info .name{
    font-weight: 500;
}

.subscribe{
    height: 100%;
    width: auto;
}

.btn-subscribe:hover{
    background-color: #333;
    color : #fff;
    cursor: pointer;
}

.btn-subscribe{
    padding : 8px 16px;
    font-weight: 600;
    border-radius: 20px;
    position: absolute;
    top : 50%;
    right : 0;
    transform: translateY(-50%);
}

.subscribed{
    background-color: #333 !important;
    color : #fff !important;
}


.header::after{
    content: '';
    display: block;
    clear : both;
}

.header span{
    float : left;
    width : auto;
    max-width: 96px;
    height : 36px;
    text-align: center;
    margin-right :  96px;
    font-size: 20px;
    line-height: 36px;
    padding-bottom: 3px;
    color : rgb(149, 135, 135);
}

.selected {
    border-bottom: 3px solid #fff;
    color : #fff !important;
}

.header span:hover{
    cursor: pointer;
    color : #fff;
}

.content{
    margin-top : 32px;
    cursor : pointer;
}

.video::after{
    content : '';
    display: block;
    clear: both;
}

.image-video{
    width: 250px;
    height : 125px;
    margin-right : 5%;
    border-radius: 15px;
}

.image-video:hover{
    cursor: pointer;
    opacity: 0.6;
}

.video{
    margin : 48px 0;
}

.video::after{
    content : '';
    display: block;
    padding : 16px 0;
    border-bottom : 1px solid #333;
}

.video p{
    font-size: 18px;
}
.video .check{
    --size : 17px;
    width: var(--size);
    height: var(--size);
    margin-top : -1px;
}
.video i{
    font-size : 12px;
}

.video .view{
    margin-left : 3%;
}

.video .description{
    max-width: 60%;
    text-align: left;
}
</style>