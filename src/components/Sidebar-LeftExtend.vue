<template>
    <div class="sidebarExtend">
        <div class="block">
            <div class="home" v-bind:class="[{active : selected === 0}]" @click="change(0)">
                <i class="ti-home"></i>
                <span>Home</span>
            </div>
            <div class="short" v-bind:class="[{active : selected === 1}]" @click="change(1)">
                <i class="ti-video-clapper"></i>
                <span>Shorts</span>
            </div>
            <div class="sub" v-bind:class="[{active : selected === 2}]" @click="change(2)">
                <i class="ti-user"></i>
                <span>Subscriptions</span>
            </div>
            </div>
        <hr>
        <div class="block">
            <div class="library" v-bind:class="[{active : selected === 99}]" @click="change(99)">
                <i class="ti-gallery"></i>
                <span>Library</span>
            </div>
            <div class="history" v-bind:class="[{active : selected === 4}]" @click="change(4)">
                <i class="ti-pie-chart"></i>
                <span>History</span>
            </div>
            <div class="your-video" v-bind:class="[{active : selected === 5}]" @click="change(5)">
                <i class="ti-control-play"></i>
                <span>Your videos</span>
            </div>
            <div class="watch-later" v-bind:class="[{active : selected === 6}]" @click="change(6)">
                <i class="ti-time"></i>
                <span>Watch later</span>
            </div>
            <div class="liked-videos" v-bind:class="[{active : selected === 7}]" @click="change(7)">
                <i class="ti-thumb-up"></i>
                <span>Liked videos</span>
            </div>
        </div>
        <hr>
        <div class="block">
            <p class="title">Subscriptions</p>
            <div v-for="id_subscribe of user.subscribe" :key="list_users[id_subscribe].author" style="padding :0;margin : 0;">
                <div v-bind:class="[{active : selected == 3 && selectchannel === list_users[id_subscribe].author}]" @click="selectChannel(list_users[id_subscribe].author)">
                    <template v-for="channel of list_users" >
                        <img v-if="(channel.author == list_users[id_subscribe].author)" :src='("/assets/img/avatar/"+list_users[id_subscribe].avatar)' :key="channel.author">
                    </template>
                    <span>{{ compact_channel(list_users[id_subscribe].author) }}</span>
                </div>
            </div>  
        </div>
        <hr>
        <div class="block">
            <p class="title">Explore</p>
            <div class="trending" v-bind:class="[{active : selected === 11}]" @click="change(11)">
                <i class="ti-bar-chart"></i>
                <span>Trending</span>
            </div>
            <div class="music" v-bind:class="[{active : selected === 12}]" @click="change(12)">
                <i class="ti-music"></i>
                <span>Music</span>
            </div>
            <div class="gaming" v-bind:class="[{active : selected === 13}]" @click="change(13)">
                <i class="ti-game"></i>
                <span>Gaming</span>
            </div>
            <div class="news" v-bind:class="[{active : selected === 14}]" @click="change(14)">
                <i class="ti-star"></i>
                <span>News</span>
            </div>
            <div class="sports" v-bind:class="[{active : selected === 15}]" @click="change(15)">
                <i class="ti-cup"></i>
                <span>Sports</span>
            </div>
        </div>
        <hr>
        <div class="block">
            <div class="settings" v-bind:class="[{active : selected === 16}]" @click="change(16)">
                <i class="ti-settings"></i>
                <span>Settings</span>
            </div>
            <div class="report" v-bind:class="[{active : selected === 17}]" @click="change(17)">
                <i class="ti-flag"></i>
                <span>Report history</span>
            </div>
            <div class="help" v-bind:class="[{active : selected === 18}]" @click="change(18)">
                <i class="ti-help-alt"></i>
                <span>Help</span>
            </div>
            <div class="feedback" v-bind:class="[{active : selected === 19}]" @click="change(19)">
                <i class="ti-comment-alt"></i>
                <span>Send feedback</span>
            </div>
        </div>
        <div class="lastBlock"></div>
    </div>
</template>
    
<script>
    export default{
        name : 'Sidebar_LeftExtend',
        data() {
            return {
                selected : 0,
                selectchannel : ''
            }
        },
        props: {
            idSelect : Number,
            user : Object,
            list_users : Array,
            authorView : String
        },
        created : function(){
            this.selected = this.idSelect
        },
        methods : {
            change : function(number){
                this.selected = number
            },
            selectChannel : function(value){
                this.selectchannel = value
                this.selected = 3 // 3 là kích hoạt xem profile của page khác để hiện view ở Channel trong content-main
                this.$emit('update:authorView',value)
            },
            compact_channel : function(value){
                if(value.length > 17) return value.substring(0,14) + '...'
                return value
            }
        },
        watch : {
            selected : function(){
                this.$emit('update:idSelect',this.selected)
            }
        }
            
    }
</script>
    
<style scoped>
*{
    user-select: none;
}

div{
    color : white;
}

.lastBlock{
    height: 200px;
}

.block{
    margin : 16px 0 ;
    text-align: left;
}

.block div{
    position: relative;
    padding : 8px 0; 
    width: 100%;
    margin-top : 10px;
}

.block div:hover{
    background-color: rgba(255, 255, 255, 0.1);
    border-radius: 10px;
    cursor: pointer;
}

.block span{
    margin-top : 8px;
    font-size: 16px;
    margin-left : 35%;
}

.block p.title{
    margin-left : 10%;
    font-size: 18px;
}

.block div i{
    position: absolute;
    left : 10%;
    font-size: 20px;
}
.block div img{
    --size : 28px;
    position: absolute;
    border-radius: 50%;
    top : 50%;
    transform: translateY(-50%);
    left : 10%;
    width: var(--size);
    height: var(--size);
}

i, p{
    color : inherit;
}

.sidebarExtend{
    padding : 0 5px;
}

hr{
    height: 1px;
    margin : 8px 0;
    background-color: #ccc;
    opacity: 0.3;
    border: none;
}

.sidebarExtend::after{
    content: '';
    display: block;
    clear: both;
}

.active{
    color : red !important;
    background-color: rgba(255, 255, 255, 0.1);
    border-radius: 10px;
}
</style>