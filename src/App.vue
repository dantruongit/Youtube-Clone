<template>
  <div id="app">
  <Header id="header" 
  :isLogin="isLogin"
  :search.sync="filter" 
  :is-extend.sync = "isExtend"
  :notification="notification"
  :idSelect.sync="idSelect"
  :showSidebar.sync="showSidebar"
  :avatar="user.avatar"
  :dataVideo="dataVideo"
  :list_users="authors"
  :key="keyHeader">
  </Header>

  <SideBar id="sidebar" :idSelect.sync = "idSelect" v-if="(isExtend == false && showSidebar==true)" :key="keySidebar"></SideBar>
  <SideBarExtend id="sidebarextend" 
    :idSelect.sync = "idSelect" 
    :key="keySidebar"
    :user="user"
    :list_users="authors"
    :authors="authors"
    :authorView.sync = "authorView"
    v-if="(isExtend == true && showSidebar==true)">
  </SideBarExtend>

  <ContentMain id="content" :class="[{contentZoomOut : (isExtend == true && showSidebar==true)},{full : showSidebar == false}]" 
    :apiRoot="apiRoot"
    :isLogin.sync = "isLogin"
    :data="data" 
    :dataVideo="dataVideo"
    :key="keyMain"
    :idSelect="idSelect"
    :shorts="short"
    :author="channel"
    :showSidebar.sync="showSidebar"
    :playing.sync="playing"
    :videoPlayer="videoPlayer"
    :user.sync = "user"
    :authorView.sync="authorView"
    :list_users="authors"
    :isreload.sync="forceReload">
  </ContentMain>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import SideBar from './components/Sidebar-Left.vue'
import SideBarExtend from './components/Sidebar-LeftExtend.vue'
import ContentMain from './components/Content-Main.vue'


export default {
  name: 'App',
  components: {
    Header,
    SideBar,
    ContentMain,
    SideBarExtend
  },
  watch:{
    filter : function(){
      var self = this
      var search = this.filter
      self.data = []
      if(search == ''){
          this.idSelect = 0
          self.keyMain = Math.ceil(Math.random() * 1000)%123 + ''
      }
      for(var item of self.dataVideo)
      {
        if(item !== undefined && (item.name.toLowerCase().startsWith(search.toLowerCase(),0)==true || item.tag.map((item)=>{
          return item.toLowerCase()
        }).includes(search.toLowerCase()) == true || self.authors[item.author].author.toLowerCase().startsWith(search.toLowerCase())==true))
          self.data.push(item)
      }
      this.keyMain = Math.ceil(Math.random() * 1000)%123 + ''
      this.idSelect = 0
      this.keySidebar = Math.ceil(Math.random() * 1000)%123 + ''
    },
    isExtend : function(){
      this.keySidebar = Math.ceil(Math.random() * 1000)%123 + ''
    },
    idSelect : function(){
      var self = this
      switch(this.idSelect){
        case -1 : {
          this.showSidebar = true
          self.keyMain = Math.ceil(Math.random() * 1000)%123 + ''
          this.keySidebar = Math.ceil(Math.random() * 1000)%123 + ''
          break
        }
        case 0 : {
          this.keySidebar = Math.ceil(Math.random() * 1000)%123 + ''
        }
      }
      self.keyMain = Math.ceil(Math.random() * 1000)%123 + ''
    },
    showSidebar : function(){
      this.keyMain = Math.ceil(Math.random() * 1000)%123 + ''
      this.keyHeader = Math.ceil(Math.random() * 1000)%123 + ''
    },
    playing: function(){
        var self = this
        self.dataVideo.forEach(function(item){
            if(item.name == self.playing.video){
                self.videoPlayer.iframe = item.iframe
                self.videoPlayer.title = item.name
                self.videoPlayer.description = item.description
                self.videoPlayer.view = item.view
                self.videoPlayer.time = item.time
            }
        })
        self.videoPlayer.channel = Object.assign({},this.authors[parseInt(self.playing.author)])
        self.keyMain = Math.ceil(Math.random() * 1000)%123 + ''
    },
    authorView : function(){ //Watch này để lắng nghe khi mà thay đổi profile định xem trong sidebar-extend
      var self = this
      this.showSidebar = true
      this.idSelect = 3
      this.channel = this.authors.find(function(channel){
          return self.authorView == channel.author
        })
      this.keyMain = Math.ceil(Math.random() * 1000)%123 + ''
    },
    user : function(){
      this.keySidebar = Math.ceil(Math.random() * 10000)%123 + ''
    },
    isLogin : function(){
      this.idSelect = 0
    },
    forceReload : function(){
      this.reload()
    }
  },
  methods:{
    loadData : function(api,callback,options){
        fetch(api,options)
          .then(function(response){
            return response.json()
          })
          .then(callback)
    },
    reload : function(){
      console.log("Hàm reload đã chạy")
      var self = this
      this.dataVideo = []
      this.data = []
      this.loadData(this.apiRoot + 'videos',function(videos){
        videos.forEach(function(video){
          self.dataVideo.push(video)
          self.data.push(video)
        })
      })
      this.keyMain = Math.ceil(Math.random() * 1000)%123 + ''
    }
  },
  created: function() {
    var self = this
    this.loadData(this.apiRoot+'notifications',function(notifications){
        notifications.forEach((noti)=>{
          self.notification.push(noti)
        })
    })
    
    this.loadData(this.apiRoot+'shorts',(shorts)=>{
        shorts.forEach((short)=>{
          self.short.push(short)
        })
    })
    this.loadData(this.apiRoot+'users',(users)=>{
      users.forEach((user)=>{
        self.authors.push(user)
      })
    })
    this.loadData(this.apiRoot + 'videos',function(videos){
        videos.forEach(function(video){
          self.dataVideo.push(video)
          self.data.push(video)
        })
    })
    this.keyMain = Math.ceil(Math.random() * 1000)%123 + ''
  }
  ,
  data(){
    return {
      apiRoot : 'http://localhost:3000/' ,
      isLogin : false, //Kiểm tra xem user đã login vào hệ thống chưa
      idSelect : 0,
      showSidebar : true,
      isExtend : false,
      keyMain : '',
      keySidebar : 'keySidebar',
      keyHeader : 'keyHeader',
      forceReload : 'ioioio', // Báo hiệu cho vue biết khi nào cần fetch lại api video mới
      data : [], //Data video truyền vào home
      authorView : '', //Channel khi bấm vào xem ở Sidebar-extend
      filter : 'all',
      user : {
        "taikhoan" : "",
        "matkhau" : "",
        "author":"",
        "avatar":"user.jpg",
        "check":false,
        "username":"",
        "subscribers":"",
        "subscribe":[],
        "videos":[],
        "banner":""
      },
      playing : {
          author : '',
          video : ''
      },
      videoPlayer:{
        iframe : '',
        title : '',
        description : '',
        view : '',
        time : '',
        channel : {
            author : '',
            avatar : '',
            check : false,
            subscribers : ''
        }
      },
      authors : [

      ],
      channel : '',
      notification : [],
      dataVideo : [],
      short: []
    }
  }
}
</script>

<style>
*{
   margin : 0;
   padding : 0;
}

html {
    scrollbar-width: none; 
    -ms-overflow-style: none; 
}

html::-webkit-scrollbar {
    width: 0px; 
}

.full{
    width: 100% !important;
    margin-left : 0 !important;
}

#app {
  overflow: scroll;
  -ms-overflow-style: none;
  scrollbar-width: none;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#app::-webkit-scrollbar{
  display : none;
}

#header{
  width: 100vw;
  height : 60px;
  background-color : #0f0f0f;
  z-index: 1;
  position : fixed;
  top : 0;
}

#sidebar{
  position: fixed;
  top : 55px;
  width: 5%;
  height: 100vh;
  background-color: #0f0f0f;
}

@keyframes extendSidebar{
  from{
    width: 5%;
  }
  to{
    width: 15%;
  }
}

#sidebarextend{
  position: fixed;
  overflow-y: hidden;
  overflow-y: scroll;
  top : 55px;
  width: 15%;
  height: 100%;
  background-color: #0f0f0f;
  overflow: auto;
  -ms-overflow-style: none;
  animation: extendSidebar ease 0.0s;
}

#sidebarextend::-webkit-scrollbar{
  display: none;
}

.contentZoomOut{
  margin-left : 15% !important;
  width: 83% !important;
}

#content{
  position: fixed;
  z-index: 0;
  margin-left : 5%;
  margin-top : 0px;
  /* margin-top : 60px; */
  width: 93%;
  height: 100%;
  background-color: #0f0f0f;
  padding : 32px 48px;
  min-height: 100vh;
  overflow-y: auto;
}

#content::after{
  content: '';
  display : block;
  margin-bottom: 200px;
}

</style>
