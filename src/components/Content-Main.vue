<template>
    <div>
        <template v-if="(showSidebar == false)">
            <div class="video-player">
                <VideoPlayer
                :iframe="videoPlayer.iframe"
                :title="videoPlayer.title"
                :description="videoPlayer.description"
                :view="videoPlayer.view"
                :time="videoPlayer.time"
                :channel="videoPlayer.channel"
                :dataVideo="list_video"
                :playing.sync="playingChild"
                :user.sync="userChild"
                :authorView.sync = "selfAuthorView"
                ></VideoPlayer>
            </div>
        </template>
        <template v-else>
            <div class="user" v-if="(idSelect == -1)">
                <ChannelYoutube 
                    :currentUser="user.author"
                    :channel="user"
                    :playing.sync ="playingChild">
                </ChannelYoutube>
            </div>

            <div class="page-home" v-if="(idSelect == 0)">
                <div class="categories">
                    <div class="category" v-for="i in tag.length > 7 ? 7 : tag.length" :key="i" :class="[{categoryActive : index == i-1}]" @click="filterbyTag(i-1,tag[i-1])">
                        {{ tag[i-1] }}
                    </div>
                </div>
                <template v-if="great4">
                <div class="rows" v-for="idx in Math.ceil(this.length/4)" v-bind:key="idx">
                    <div v-for="i in 4" :key="i" @click="playVideo(data[i-1+(idx-1)*4].author,data[i-1+(idx-1)*4].name)">
                        <VideoComponent   
                        :name = "data[i-1+(idx-1)*4].name"
                        :image = "data[i-1+(idx-1)*4].image"
                        :avatar = "data[i-1+(idx-1)*4].avatar"
                        :view="data[i-1+(idx-1)*4].view" 
                        :time="data[i-1+(idx-1)*4].time"
                        :author="data[i-1+(idx-1)*4].author"
                        :check="data[i-1+(idx-1)*4].check"
                        :link="data[i-1+(idx-1)*4].link"
                        :iframe="data[i-1+(idx-1)*4].iframe"
                        :gif="data[i-1+(idx-1)*4].gif"
                        v-if="data[i-1+(idx-1)*4]"
                        ></VideoComponent>
                    </div>
                </div>
                </template>
                <div class="rows" v-if="great4===false">
                    <div v-for="i in 4" :key="i" @click="playVideo(data[i-1+(idx-1)*4].author,data[i-1+(idx-1)*4].name)">
                        <VideoComponent           
                        :name = "data[i-1].name"
                        :image = "data[i-1].image"
                        :avatar = "data[i-1].avatar"
                        :view="data[i-1].view" 
                        :time="data[i-1].time"
                        :author="data[i-1].author"
                        :check="data[i-1].check"
                        :link="data[i-1].link"
                        :gif="data[i-1].gif"
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
                <p class="title-subscription">This year</p>
                <template v-if="(length > 5)">
                <div class="rows" v-for="idx in Math.ceil(this.length/4)" v-bind:key="idx">
                    <div v-for="i in 5" :key="i" @click="playVideo(data[i-1+(idx-1)*5].author,data[i-1+(idx-1)*5].name)">
                        <VideoSubscript        
                        :name = "data[i-1+(idx-1)*5].name"
                        :image = "data[i-1+(idx-1)*5].image"
                        :view="data[i-1+(idx-1)*5].view" 
                        :time="data[i-1+(idx-1)*5].time"
                        :author="data[i-1+(idx-1)*5].author"
                        :check="data[i-1+(idx-1)*5].check"
                        :link="data[i-1+(idx-1)*5].link"
                        :gif="data[i-1+(idx-1)*5].gif"
                        v-if="data[i-1+(idx-1)*5]"
                        ></VideoSubscript>
                    </div>
                </div>
                </template>
                <div class="rows" v-else>
                    <div v-for="i in 5" :key="i" @click="playVideo(data[i-1+(idx-1)*5].author,data[i-1+(idx-1)*5].name)">
                        <VideoSubscript          
                        :name = "data[i-1].name"
                        :image = "data[i-1].image"
                        :view="data[i-1].view" 
                        :time="data[i-1].time"
                        :author="data[i-1].author"
                        :check="data[i-1].check"
                        :link="data[i-1].link"
                        :gif="data[i-1].gif"
                        v-if="data[i-1]"
                        ></VideoSubscript>
                    </div>
                </div>
            </div>

            <div class="channel" v-if="(idSelect == 3)">
                <ChannelYoutube 
                    :currentUser="user.author"
                    :channel="author"
                    :playing.sync ="playingChild"
                    :user.sync="userChild">
                </ChannelYoutube>
            </div>
            <div class="page-else" v-else>
            </div>
        </template>
    </div> 
</template>
<script>
    import VideoComponent from './VideoComponent.vue'
    import VideoShort from './VideoShort.vue'
    import VideoSubscript from './VideoSubscriptions.vue'
    import ChannelYoutube from './ChannelYoutube.vue'
    import VideoPlayer from './VideoPlayer.vue'

    export default{
        name : 'ContentMain',
        data(){
            return {
                indexShort : 0,
                length : 0,
                great4 : false,
                list_video : [],
                playingChild : {
                    author : '',
                    video : ''
                },
                userChild : {},
                selfAuthorView : ''
            }
        },
        props:{
            user : {
                name : String,
                avatar : String,
                check : Boolean,
                username : String,
                subscribers : String,
                subscribe : Array,
                videos : Array,
                banner : String
            },
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
            videoPlayer : {
                iframe : String,
                title : String,
                description : String,
                channel : {
                    author : String,
                    avatar : String,
                    check : Boolean,
                    subscribers : String
                }
            },
            authorView : String
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
            this.length = this.data.length
            this.great4 = this.length > 4
            this.dataVideo.forEach(function(item){
                self.list_video.push(item)
            })
            this.list_video = this.list_video.sort( () => .5 - Math.random());
            for(var key in this.user){ //Copy user to childUser
                this.userChild[key] = this.user[key]
            }
        },
        components : {
            VideoComponent,
            VideoShort,
            VideoSubscript,
            ChannelYoutube,
            VideoPlayer
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