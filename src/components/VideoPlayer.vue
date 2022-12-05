<template>
    <div>
        <div class="left">
           <iframe class="video" :src="iframe" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
           <h2 style="width : 100%; text-align: left; font-weight: 550;">{{ compact_title }}</h2>
           <div class="owner">
                <img class="avatar" :src="link_image" alt="" @click="viewAuthor()" style="cursor:pointer;">
                <span class="author" @click="viewAuthor()" style="cursor:pointer;">{{channel.author}}</span>
                <div class="check"  v-if="channel.check">
                    <i class="ti-check"></i>
                </div>
                <button class="btn btn-subscribe" v-if="(user.author !== channel.author)" :class="[{subscribed : (isSubscribe == 'Subscribed')}]" @click="follow()">{{isSubscribe}}</button>
                <br> <br>
                <span class="sub blur-text">{{channel.subscribers}} subscribers</span>
           </div>
           <div class="description">
            <p>{{view }} views • {{time}} ago </p><br> <br>
                {{description}}
           </div>
        </div>
        <div class="right">
            <div class="suggest" v-for="(item,index) in dataVideo" :key="item.index" @click="playVideo(item.author,item.name)">
                <template v-if="(index<10)">
                    <img class="video" :src="link_video(item.image)" alt="">
                    <div>{{ compact_name(item.name)}} </div>
                    <div class="info">
                        <p class="blur-text">{{item.author}}</p> <br>
                        <p class="blur-text">{{item.view}} views . {{item.time}} ago</p>
                    </div>
                </template>
            </div>
        </div>
    </div>
</template>
<script>
export default{
    name : 'VideoPlayer',
    data(){
        return {
            isSubscribe : 'Subscribe',
            selfUser : {
                
            }
        }
    },
    created : function(){
        for(var key in this.user){
            this.selfUser[key] = this.user[key]
        }
        if(this.selfUser.subscribe.includes(this.channel.author)){
            this.isSubscribe = 'Subscribed'
        }
    },
    props:{
        iframe : String,
        title : String,
        description : String,
        view : String,
        time : String,
        channel : {
            author : String,
            avatar : String,
            check : Boolean,
            subscribers : String
        },
        dataVideo : Array,
        playing: {
            author : String,
            video : String
        },
        user : {
            author : String,
            avatar : String,
            check : Boolean,
            username : String,
            subscribers : String,
            subscribe : Array,
            videos : Array,
            banner : String
        },
        authorView : String
    },
    methods: {
        link_video : function(value){
            return '/assets/img/video/' + value
        },
        compact_name : function(value){
            if(value.length > 62) return value.substring(0,59) + '...'
            return value
        },
        playVideo : function(Author,videoName){
            var playnow = {
                author:  Author,
                video : videoName
            }
            this.$emit('update:playing',playnow)
        },
        follow : function(){
            this.isSubscribe = this.isSubscribe=='Subscribe'?'Subscribed':'Subscribe'
            if(this.isSubscribe == 'Subscribed'){
                if(this.selfUser.subscribe.indexOf(this.channel.author) == -1) this.selfUser.subscribe.push(this.channel.author)
            }
            else {
                if(this.selfUser.subscribe.indexOf(this.channel.author) != -1){
                    this.selfUser.subscribe.splice(this.selfUser.subscribe.indexOf(this.channel.author),1)
                }
            }
        },
        viewAuthor : function(){
            this.$emit('update:authorView',this.channel.author)
        }
    },
    computed : {
        compact_title : function(){
            if(this.title.length > 70) return this.title.substring(0,70) + '...'
            return this.title
        },
        link_image : function(){
            return '/assets/img/avatar/' + this.channel.avatar
        },
        
    },
    watch : {
        selfUser : function(){
            console.log("Update user video-player")
            this.$emit('update:user',this.selfUser)
        }
    }
}
</script>
<style scoped>

.blur-text{
    opacity: 0.6;
    font-size: 15px;
    color : rgb(149, 135, 135);
}
.left{
    width: 62%;
    float : left;
    box-sizing: border-box;
}

.right{
    box-sizing: border-box;
    padding: 0 16px;
    width: 35%;
    height: 100%;
    float : right;
}

.right::after{
    content: '';
    display: block;
    margin-bottom: 250px;
    clear : both;
}


.left::after,.suggest::after{
    content: '';
    display: block;
    margin-bottom: 500px;
    clear : both;
}


.left *{
    float : left;
    margin-bottom: 12px;
    color : #fff;
}

.left .video{
    width : 956px; 
    height : 538px;
}
.owner{
    display: inline-block;
    width: 100%;
    position: relative;
}

.avatar{
    --size : 48px;
    width: var(--size);
    height: var(--size);
    border-radius: 50%;
}

.author{
    font-weight: bold;
    margin-left : 24px;
    margin-bottom: 0;
}

.check{
    --size : 17px;
    width: var(--size);
    height: var(--size);
    margin-top : -1px;
    border-radius: 50%;
    background-color: rgb(149, 135, 135) !important;
    text-align: center;
    margin-left : 10px;
}

i.ti-check{
    font-size : 12px;
    line-height: var(--size);
    padding-left : 2px;
}
.sub{
    margin-left : 24px;
}
.btn, .react{
    float :none;
    padding : 8px 16px;
    font-weight: 600;
    border-radius: 20px;
    transform: translateY(-50%);
    color : #333;
    position: absolute;
    top : 50%;
}

.btn:hover{
    background-color: #333;
    color : #fff;
    cursor: pointer;
}

.react{
    position: absolute;
    top : 50%;
    left : 50%;
}

.react button{
    background-color : #333;
    color : #fff;
    padding : 8px 24px;
    font-weight: 500;
    line-height: 27px;
    border : none;
}

.react button:hover, .react button:hover i{
    background-color: #fff;
    color : #333;
    cursor: pointer;
}

.react button.like{
    border-radius: 20px 0 0 20px;
    padding : 8px 36px;
    border-right: 0.01rem solid #fff;
}

.react button.dislike{
    border-radius: 0px 20px 20px 0 ;
}

.react i{
    margin-top : 5px;
}


.btn-subscribe{
    left : 30%
}

.subscribed{
    background-color: #333 !important;
    color : #fff !important;
}

button.ti-share{
    position: absolute;
    right : -50%;
    padding : 10px 24px;
    border-radius: 20px;
}

div.description{
    background-color: rgba(255, 255, 255, 0.1);
    width: 100%;
    max-height: 500px;
    min-height: 175px;
    border-radius : 20px;
    text-align: left;
    padding : 16px 8px;
}

.suggest{
    max-height: 150px;
    --left : 12px;
}

.suggest:hover{
    cursor: pointer;
}
.suggest *{
    float : left;
}

.suggest .video{
    height: 100px;
    width: 200px;
    border-radius: 15px;
    margin-bottom: 32px;
    margin-right : 16px;
}
.suggest .video:hover{
    opacity: 0.6;
}
.suggest div{
    color : #fff;
    float : none;
    text-align: left;
    padding-right : 32px;
}   

.suggest .info{
    box-sizing: border-box;
    margin-top: 20px;
}

</style>