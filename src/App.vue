<template>
  <div id="app">
  <Header id="header" 
  :search.sync="filter" 
  :is-extend.sync = "isExtend"
  :notification="notification"
  :idSelect.sync="idSelect"
  :showSidebar.sync="showSidebar"
  :avatar="user.avatar"
  :key="keyHeader">
  </Header>

  <SideBar id="sidebar" :idSelect.sync = "idSelect" v-if="(isExtend == false && showSidebar==true)" :key="keySidebar"></SideBar>
  <SideBarExtend id="sidebarextend" 
    :idSelect.sync = "idSelect" 
    v-if="(isExtend == true && showSidebar==true )" 
    :key="keySidebar"
    :user="user"
    :list_users="authors"
    :authors="authors"
    :authorView.sync = "authorView">
  </SideBarExtend>

  <ContentMain id="content" :class="[{contentZoomOut : (isExtend == true && showSidebar==true)},{full : showSidebar == false}]" 
    :apiRoot="apiRoot"
    :isLogin.sync = "isLogin"
    :data="data" 
    :dataVideo="dataVideo"
    :filterTag.sync="filterTag" 
    :key="keyMain"
    :index.sync="indexTag"
    :tag="tag"
    :idSelect="idSelect"
    :shorts="short"
    :author="channel"
    :showSidebar.sync="showSidebar"
    :playing.sync="playing"
    :videoPlayer="videoPlayer"
    :user.sync = "user"
    :authorView.sync="authorView"
    :list_users="authors">
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
          this.indexTag = 0
          this.idSelect = 0
          self.keyMain = Math.ceil(Math.random() * 1000)%123 + ''
      }
      for(var item of self.dataVideo)
      {
        if(item !== undefined && (item.name.toLowerCase().startsWith(search.toLowerCase(),0)==true || item.tag.map((item)=>{
          return item.toLowerCase()
        }).includes(search.toLowerCase()) == true))
          self.data.push(item)
      }
      this.keyMain = Math.ceil(Math.random() * 1000)%123 + ''
      this.indexTag = 0
      this.idSelect = 0
      this.keySidebar = Math.ceil(Math.random() * 1000)%123 + ''
    },
    filterTag : function(){
      var self = this
      this.filter = ''
      self.data = []
      for(var item of self.dataVideo)
      {
        if(item !== undefined && item.tag.includes(self.filterTag))
          self.data.push(item)
      }
      self.keyMain = Math.ceil(Math.random() * 1000)%123 + ''
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
    convertToUser : function(str){
        str = str.toLowerCase();
        str = str.replace(/à|á|ạ|ả|ã|â|ầ|ấ|ậ|ẩ|ẫ|ă|ằ|ắ|ặ|ẳ|ẵ/g, "a");
        str = str.replace(/è|é|ẹ|ẻ|ẽ|ê|ề|ế|ệ|ể|ễ/g, "e");
        str = str.replace(/ì|í|ị|ỉ|ĩ/g, "i");
        str = str.replace(/ò|ó|ọ|ỏ|õ|ô|ồ|ố|ộ|ổ|ỗ|ơ|ờ|ớ|ợ|ở|ỡ/g, "o");
        str = str.replace(/ù|ú|ụ|ủ|ũ|ư|ừ|ứ|ự|ử|ữ/g, "u");
        str = str.replace(/ỳ|ý|ỵ|ỷ|ỹ/g, "y");
        str = str.replace(/đ/g, "d");
        str = str.replace(/\u0300|\u0301|\u0303|\u0309|\u0323/g, ""); // Huyền sắc hỏi ngã nặng 
        str = str.replace(/\u02C6|\u0306|\u031B/g, ""); // Â, Ê, Ă, Ơ, Ư
        return str.replaceAll(' ','');
    }
  },
  created: function() {
    var self = this
    //'https://638cbbbfeafd555746ad5623.mockapi.io/api/v1/'
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
      apiRoot : 'https://638cbbbfeafd555746ad5623.mockapi.io/api/v1/' ,
      isLogin : false, //Kiểm tra xem user đã login vào hệ thống chưa
      indexTag : 0,
      tag : ['All','Music'],
      idSelect : 0,
      showSidebar : true,
      isExtend : false,
      keyMain : '',
      keySidebar : 'keySidebar',
      keyHeader : 'keyHeader',
      data : [], //Data video truyền vào home
      authorView : '', //Channel khi bấm vào xem ở Sidebar-extend
      filter : 'all',
      filterTag : 'all',
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
