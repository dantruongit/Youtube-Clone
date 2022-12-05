<template>
  <div class="header">
    <i class="ti-menu" @click="extend()" v-if="(showSidebar==true)"></i>
    <div class="logo">
      <svg @click="home()" class="yt-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 576 512"><path d="M549.655 124.083c-6.281-23.65-24.787-42.276-48.284-48.597C458.781 64 288 64 288 64S117.22 64 74.629 75.486c-23.497 6.322-42.003 24.947-48.284 48.597-11.412 42.867-11.412 132.305-11.412 132.305s0 89.438 11.412 132.305c6.281 23.65 24.787 41.5 48.284 47.821C117.22 448 288 448 288 448s170.78 0 213.371-11.486c23.497-6.321 42.003-24.171 48.284-47.821 11.412-42.867 11.412-132.305 11.412-132.305s0-89.438-11.412-132.305zm-317.51 213.508V175.185l142.739 81.205-142.739 81.201z"/></svg>
      <h2  @click="home()" class="yt-title">Youtube <span>VN</span></h2>
    </div>
    
    <div class="search-bar">
      <i class="ti-search search-icon"></i>
      <input type="text" class="input-bar" placeholder="Search" v-model="keyword">
      <button title="Search" class="btn-search-bar" @click="change()"><i class="ti-search"></i></button>
    </div>

    <div class="user">
      <div class="img" @click="user()">
        <img :src="link_image(avatar)" alt="" width="32px" height="32px">
      </div>
      <!-- Notifications -->
      <i title="Notifications" class="ti-bell" :class="[{active : formNoti},{formShow : formNoti == true}]" @click="show('noti')">
        <div class="form noti" :class="[{show : formNoti==true}]">
          <div class="row" v-for="item of notification" :key="item.content">
            <div class="userNoti">
              <img :src="link_image(item.avatar)" alt="">
            </div>
            <div class="content">
              <!-- Max 98 length ...  -->
              <div class="content-noti" :class="[{strong : item.content.includes('global')}]">
                  {{ compact_content(item.content) }}
              </div>
              <p class="content-time">{{ item.time }}</p>
            </div>
            <div class="video">
              <img :src="link_video(item.img)" alt="" v-if="item.img!=''" >
            </div> 
          </div>
        </div>
      </i>
      <!-- Upload video -->
      <i title="Create" class="ti-video-camera" :class="[{active : formUpload},{formShow : formUpload == true}]" @click="show('upload')">
        <div class="form upload" :class="[{show : formUpload==true}]">
          <div class="row">
            <i class="ti-upload"></i>
            <span>Upload video</span>
          </div>
          <div class="row">
            <i class="ti-eye"></i>
            <span>Go live</span>
          </div>
        </div>
      </i>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    search : String,
    isExtend : Boolean,
    notification : Array,
    idSelect : Number,
    showSidebar : Boolean,
    avatar : String
  },
  data(){
    return {
        keyword : '',
        formNoti : false,
        formUpload : false,
    }
  },
  methods:{
    extend : function(){
      this.$emit('update:isExtend', !this.isExtend)
    },
    change : function(){
      this.$emit('update:search', this.keyword)
    },
    home : function(){
        this.$emit('update:idSelect',0)
        this.$emit('update:showSidebar',true)
    },
    user : function(){
      this.$emit('update:idSelect',-1)
    },
    show : function(value){
      if(value == 'upload'){
        this.formUpload = !this.formUpload
        this.formNoti = false
      }
      else{
        this.formNoti = !this.formNoti
        this.formUpload = false
      }
    },
    link_image : function(value){
        return '/assets/img/avatar/' +value
    },
    link_video : function(value){
      return '/assets/img/video/' +value
    },
    compact_content : function(value){
      if(value.length > 98){
        return value.substring(0,95).replace('global','') + '...'
      }
      return value.replace('global','')
    }
  },
  watch:{
      keyword : function(){
        if(this.keyword == '') this.change()
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
*{
  color : white;
}
.active{
  color : red;
}

.strong{
  font-weight: 700;
  color: green;
}

.formShow{
  background-color : rgba(255, 255, 255, 0.1);
}

.show{
  display: block !important;
}

i:hover{
  cursor : pointer;
}

i.search-icon{
  cursor: auto;
}

.header{
  position : relative;
}

i.ti-menu{
  --size : 50px;
  position : absolute;
  font-size : 24px;
  line-height: var(--size);
  float : left;
  left : 1%;
  top : 50%;
  transform : translateY(-50%);
  min-width: 42px;
  width : var(--size);
  height : var(--size);
  border-radius: 50%;
}



.logo{
  position: absolute;
  left : 6%;
  height: 100%;
  width : 200px;
}

.logo::after{
  clear: both;
}

.logo svg:hover, .logo h2{
  cursor: pointer;
}

.yt-icon{
  position: absolute;
  height : 24px;
  top : 50%;
  transform : translateY(-50%);
  fill : red;
  left : 0;
}

.yt-title{
  position: absolute;
  font-size: 20px;
  font-weight: 600;
  top : 50%;
  transform : translateY(-50%);
  left : 15%;
}
.yt-title span{
  position: absolute;
  right : -15%;
  font-size: 8px;
  font-weight: 100;
}

.search-bar{
  --radius : 40px;
  position: absolute;
  top : 50%;
  left : 50%;
  width: 35%;
  height : 70%;
  transform: translateY(-50%) translateX(-50%);
  border : 1px solid white;;
  border-radius: var(--radius);
  padding : 0 30px;
}

.search-bar i {
  float : left;
  height: 100%;
  line-height: 40px;
  padding : 0 20px 0 0;
}

.input-bar{
  background-color: rgba(0,0,0,0);
  height: 100%;
  width : 80%;
  font-size: 16px;
  border : none;
  line-height: 100%;
  float : left;
}

.input-bar:focus{
  outline: none;
}

.btn-search-bar{
  position: absolute;
  height: 100%;
  width: 15%;
  line-height: 100%;
  background-color: hsla(0, 0%, 100%, 0.08);
  border : 1px solid hsl(0, 0%, 18.82%);
  border-radius : 0 40px 40px 0 ;
  padding : 0 32px;
  right : 0;
  cursor: pointer;
}

.user{
  position: absolute;
  width : 30%;
  height: 100%;
  right : 0;
}
.user div.img{
  position: relative;
  height: 100%;
  width: 10%;
  float : right;
  margin-right : 24px;
}
.user div.img img{
  position: absolute;
  top : 45%;
  left : 50%;
  transform: translateY(-50%) translateX(-50%);
  right : 0;
  border-radius: 50%;
  cursor: pointer;
  /* left : -50%; */
}
i.ti-bell, i.ti-video-camera{
  --size : 50px;
  font-size : 24px;
  line-height: var(--size);
  float : right;
  width: var(--size);
  height: var(--size);
  border-radius: 50%;
}

.form{
  position: fixed;
  background-color: #212121;
  right : 8%;
  width: 200px;
  z-index: 2;
  border-radius: 8px;
  display: none;
}

.upload{
  height: 125px;
}
.upload div.row{
  box-sizing: border-box;
  text-align: left;
  position: relative;
  height: 40%;
  width: 100%;
  margin : 8px 0;
  user-select: none;
}

.upload div.row:hover{
  background-color: rgba(255, 255, 255, 0.2);
}

.upload span{
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  margin-left : 55px;
  font-weight: 400;
  font-size: 15px;
  line-height: 100%;
}

i.ti-upload,i.ti-eye{
    position: absolute;
    left : 8%;
    top : 59%;
    transform: translateY(-50%);
    font-size: 20px;
}

.noti{
  top : 8%;
  right : 5%;
  height: auto;
  width: 390px;
  min-height: 200px;
  max-height: 500px;
  overflow: auto;
  
}

.noti::-webkit-scrollbar{
  display: none;
}


.noti::before{
  position: fixed;
  box-sizing: border-box;
  border-radius: 8px 8px 0 0;
  background-color: #212121;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  padding : 0 16px;
  width : 390px;
  font-size: 15px;
  text-align: left;
  content: 'Thông báo';
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  display: block;
  z-index: 3;
}

.noti .row:first-child{
  margin-top : 50px
}

.noti::after{
  content : '';
  display: block;
  height: 50px;
}

.noti .row{
  box-sizing: border-box;
  text-align: left;
  position: relative;
  height: 135px;
  width: 100%;
  margin : 8px 0;
  user-select: none;
}

.noti .row:hover{
  background-color: rgba(255, 255, 255, 0.2);
}

.noti .userNoti{
  width: 48px;
  height: 48px;
}

.noti .userNoti img{
  position: absolute;
  width: 48px;
  left : 5%;
  top : 10%;
  height: 48px;
  border-radius: 50%;
}

.noti .content{
  position: absolute;
  left : 20%;
  top : 0;
  height: 100%;
}

.noti .content p{
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-size: 14px;
}

.noti .content .content-noti{
  padding-top : 12px;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  line-height: 20px;
  font-size: 14px;
  width: 200px;
}

.noti .content p.content-time{
  position: absolute;
  bottom : 0;
  opacity: 0.6;
  font-size: 12px;
}

.noti .video{
  position: absolute;
  top : 10%;
  right : 4%;
  width: 90px;
  height: 60px;
}

.noti .video img{
  width: 100%;
  height: 100%;
  border-radius: 8px;
}

i.ti-menu:hover,
i.ti-bell:hover,
i.ti-video-camera:hover{
  background-color : rgba(255, 255, 255, 0.1);
}
</style>
