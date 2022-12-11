<template>
    <div class="item-video" :title="video.name">
        <img v-bind:src="imgshow" v-on:mouseover="hover()" v-on:mouseleave="dishover()" alt="" >
        <div class="content-video">
            <div class="row" style="margin : 0px;">
                <div class="title col">
                    <p class="name-video">
                        {{ title }}
                    </p>
                    <div class="author">
                        {{ user.author }}
                        <div class="check"  v-if="user.check">
                            <i class="ti-check"></i>
                        </div>
                    </div>
                    <div class="view">
                        <br>
                        <p>{{ video.view }} views • {{ video.time }} ago</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    //Length title max = 64 + 3(...) . substring(0,64)
    export default{
        name : 'VideoSubscriptions',
        props: {
            video : Object,
            user : Object
        },
        computed : {
            title : function(){
                if(this.video.name.length > 61) { return this.video.name.substring(0,50) + '...'}
                else return this.video.name
            },
            link_avatar : function(){
                return "/assets/img/avatar/" + this.user.avatar
            },
            link_image : function(){
                return "/assets/img/video/" + this.video.image
            },
            link_src : function(){
                return this.link
            },
            link_gif : function(){
                return "/assets/img/video/" + this.video.gif
            }
        },
        methods : {
            navigate : function(){
                console.log(this.link);
                window.location.href = this.link
            },
            hover : function(){
                this.imgshow = this.link_gif == '' ? this.link_image : this.link_gif
            },
            dishover : function(){
                this.imgshow = this.link_image
            }
        },
        data() {
            return {
                imgshow : ''
            }
        },
        created: function(){
            this.imgshow = this.link_image
        }
    }
</script>
<style scoped>

p{
    padding-top : 4px;
    text-align: left;
    color : white;
    font-weight: 400px;
}
.row {
    margin-bottom: 40px;
}

.row::after{
    content : '';
    display: block;
    clear: both;
}

.col{
    float : left;
    padding : 0 3px;
}


.content-video{
    width: 100%;
    height : 100px;
    /* background-color: brown; */
    padding-top : 8px;
}

.item-video{
  cursor: pointer;
  float : left;
  margin : 0 8px 0 8px;
  width: calc(21% - 36px);
  height : 300px;
  /* background-color: white; */
}

.item-video > img{
    width: 100%;
    height : 200px;
    border-radius: 5%;
}

.item-video > img:hover{
    opacity: 0.6;
    border-radius: 5%;
}

.author{
    opacity: 0.6;
    float : left;
    padding-top : 4px;
    color : rgb(170,170,170)
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
    float : right;
    margin-left : 7px;
}

.view{
    padding-top : 8px;
}

.view p{
    font-size: 14px;
    opacity: 0.6;
}
</style>