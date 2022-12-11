<template>
    <div>
        <template v-if="(isLogin == false && idSelect != 0 && idSelect != 1 && idSelect != 3)">
            <div class="login">
                <LoginForm
                :user.sync="userChild"
                :apiRoot="apiRoot"
                :Connected.sync="selfisLogin">
                </LoginForm>
            </div>
        </template>
        <template v-else-if="(showSidebar == false)">
            <div class="video-player">
                <VideoPlayer
                :is-login="isLogin"
                :videoplayer="videoPlayer"
                :dataVideo="list_video"
                :playing.sync="playingChild"
                :user.sync="userChild"
                :authorView.sync = "selfAuthorView"
                :list_users="list_users"
                :api-root="apiRoot"
                ></VideoPlayer>
            </div>
        </template>
        <template v-else>
            <div class="upload-video" v-if="(idSelect == -2)">
                <UploadVideo
                :user="userChild"
                :api-root="apiRoot"
                :isreload.sync="selfReload">        
                </UploadVideo>
            </div>
            <div class="user" v-if="(idSelect == -1)">
                <ChannelYoutube 
                    :currentUser="user.author"
                    :channel="user"
                    :playing.sync ="playingChild"
                    :user.sync="userChild"
                    :list_users="list_users"
                    :dataVideo="dataVideo">
                </ChannelYoutube>
            </div>

            <div class="page-home" v-if="(idSelect == 0)">
                <template v-if="(data.length > 4)">
                <div class="rows" v-for="idx in Math.ceil(data.length/4)" v-bind:key="idx">
                    <div v-for="i in 4" :key="i" @click="playVideo(data[i-1+(idx-1)*4].author,data[i-1+(idx-1)*4].name)">
                        <VideoComponent   
                        :video="data[i-1+(idx-1)*4]"
                        :apiRoot="apiRoot"
                        :user="list_users[data[i-1+(idx-1)*4].author]"
                        v-if="data[i-1+(idx-1)*4]"
                        ></VideoComponent>
                    </div>
                </div>
                </template>
                <div class="rows" v-if="(data.length <= 4)">
                    <div v-for="i in 4" :key="i" @click="playVideo(data[i-1+(idx-1)*4].author,data[i-1+(idx-1)*4].name)">
                        <VideoComponent           
                        :video="data[i-1]"
                        :apiRoot="apiRoot"
                        :user="list_users[data[i-1].author]"
                        v-if="data[i-1]"
                        ></VideoComponent>
                    </div>
                </div>
            </div>

            <div class="page-short" v-if="(idSelect == 1)">
                <template >
                    <VideoShort 
                    v-for="item in shorts"
                    :source="item.source"
                    :like="item.like"
                    :comment="item.comment"
                    :author="item.author"
                    :avatar="item.avatar"
                    :key="item.source"
                    ></VideoShort>
                </template>
            </div>

            <div class="subscription" v-if="(idSelect == 2)">
                <p class="title-subscription">Subscribe</p>
                <template v-if="(list_subscribe.length > 5)">
                <div class="rows" v-for="idx in Math.ceil(list_subscribe.length/4)" v-bind:key="idx">
                    <div v-for="i in 5" :key="i" @click="playVideo(list_subscribe[i-1+(idx-1)*5].author,list_subscribe[i-1+(idx-1)*5].name)">
                        <VideoSubscript        
                        :video="list_subscribe[i-1+(idx-1)*5]"
                        :user="list_users[list_subscribe[i-1+(idx-1)*5].author]"
                        v-if="list_subscribe[i-1+(idx-1)*5]"
                        ></VideoSubscript>
                    </div>
                </div>
                </template>
                <div class="rows" v-else>
                    <div v-for="i in 5" :key="i" @click="playVideo(list_subscribe[i-1+(idx-1)*5].author,list_subscribe[i-1+(idx-1)*5].name)">
                        <VideoSubscript          
                        :video="list_subscribe[i-1]"
                        :user="list_users[list_subscribe[i-1].author]"
                        v-if="list_subscribe[i-1]"
                        ></VideoSubscript>
                    </div>
                </div>
            </div>

            <div class="channel" v-if="(idSelect == 3)">
                <ChannelYoutube 
                    :currentUser="user.author"
                    :channel="author"
                    :playing.sync ="playingChild"
                    :user.sync="userChild"
                    :list_users="list_users"
                    :dataVideo="dataVideo"
                    :apiRoot="apiRoot">
                </ChannelYoutube>
            </div>
            
            <div class="page-else" v-if="(idSelect > 3)">
                <MessageForm></MessageForm>
            </div>
        </template>
    </div> 
</template>
<script>
    import ChannelYoutube from './ChannelYoutube.vue'
    import LoginForm from './LoginForm.vue'
    import MessageForm from './MessageForm.vue'
    import VideoComponent from './VideoComponent.vue'
    import VideoShort from './VideoShort.vue'
    import VideoSubscript from './VideoSubscriptions.vue'
    import VideoPlayer from './VideoPlayer.vue'
    import UploadVideo from './UploadVideo.vue'

    export default{
        name : 'ContentMain',
        data(){
            return {
                selfisLogin : false,
                indexShort : 0,
                length : 0,
                list_video : [],
                playingChild : {
                    author : '',
                    video : ''
                },
                userChild : {},
                selfAuthorView : '',
                list_subscribe : [],
                selfReload : 'iok'
            }
        },
        props:{
            apiRoot : String,
            isLogin : Boolean,
            user : Object,
            author : {
                avatar : String,
                author : String,
                check : Boolean,
                username : String,
                subscribers : String,
                videos : Array,
                banner : String
            },
            msg : String,
            data : Array,
            dataVideo : Array,
            index : Number,
            filterTag : String,
            tag : Array,
            idSelect : Number,
            shorts : Array,
            showSidebar : Boolean,
            playing : {
                author : String,
                video : String
            },
            videoPlayer : Object,
            authorView : String,
            list_users : Array,
            isreload : String
        },
        watch : {
            playingChild : function(){
                this.$emit('update:playing',this.playingChild)
                this.playVideo(this.playingChild.author,this.playingChild.video)
            },
            userChild : function(){
                this.$emit('update:user',this.userChild)
            },
            selfAuthorView : function(){
                this.$emit('update:authorView',this.selfAuthorView)
            },
            selfisLogin : function(){
                this.$emit('update:isLogin',this.selfisLogin)
            },
            isreload : function(){
                console.log("self reload contentmain")
                this.$emit('update:isreload',this.selfReload)
            }
        },
        methods:{
            filterbyTag : function(idx,value){
                this.$emit('update:filterTag',value)
                this.$emit('update:index',idx)
            },
            playVideo : function(Author,videoName){
                var playnow = {
                    author:  Author,
                    video : videoName
                }
                this.$emit('update:showSidebar',false)
                this.$emit('update:playing',playnow)
            }
        },
        created : function() {
            var self = this
            this.length = this.dataVideo.length
            this.selfisLogin = this.isLogin
            this.dataVideo.forEach(function(item){
                self.list_video.push(item)
            })
            this.list_video = this.list_video.sort( () => .5 - Math.random());
            this.userChild = Object.assign({},this.user)
            this.dataVideo.forEach((video)=>{
                if(self.userChild.subscribe.includes(parseInt(video.author)))
                    this.list_subscribe.push(video)
            })
            this.selfReload = this.isreload
        },
        components : {
            LoginForm,
            VideoComponent,
            VideoShort,
            VideoSubscript,
            ChannelYoutube,
            VideoPlayer,
            MessageForm,
            UploadVideo
        }
    }
</script>
<style scoped>

/* Home area */

div.categories{
    position: fixed;
    width: 100%;
    height: 50px;
    top : 60px;
    background-color : #0f0f0f;
    z-index: 1;
}

.category{
    float : left;
    margin-top : 10px;
    margin-right : 16px;
    background-color: black;
    color : #fff;
    padding : 6px 12px;
    width : auto;
    border-radius: 8px;
    cursor: pointer;
    font-weight: 400;
}

.category:hover{
    background-color : rgba(255, 255, 255, 0.2)
}

.categoryActive{
    color : #000 !important;
    background-color: #f1f1f1 !important;
}

div.categories::after{
    content: '';
    display: block;
    clear: both;
}



.rows {
    margin: 40px 0;
}

.rows::after{
    content : '';
    display: block;
    clear: both;
}

/* Short area */



/* Subscription */
p.title-subscription{
    text-align: left;
    margin-left : 16px;
    font-size: 16px;
    color : #fff;
    font-weight: bold;
}
</style>