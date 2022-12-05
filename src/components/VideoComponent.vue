<template>
    <div class="item-video">
        <img v-bind:src="imgshow" v-on:mouseover="hover()" v-on:mouseleave="dishover()" alt="" :title="name">
        <div class="content-video">
            <div class="row" style="margin : 0px;">
                <div class="avatar col col-2">
                    <img v-bind:src="link_avatar" alt="" width="36px" height="36px" style="border-radius : 50%;">
                </div>
                <div class="title col col-8">
                    <p class="name-video">
                        {{ title }}
                    </p>
                    <div class="author">
                        {{ author }}
                        <div class="check"  v-if="check" :title="'Verified'">
                            <i class="ti-check"></i>
                        </div>
                    </div>
                    <div class="view">
                        <br>
                        <p>{{ view }} views • {{ time }} ago</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    export default{
        name : 'VideoPlayer',
        data(){
            return {
                showVideo : true,
                imgshow : ''
            }
        },
        props: {
            name : String,
            view : String,
            time : String,
            avatar : String,
            iframe : String,
            image : String,
            author : String,
            check : Boolean,
            link : String,
            gif : String,

            idSelect : Number
        },
        computed : {
            title : function(){
                if(this.name.length > 61) { return this.name.substring(0,50) + '...'}
                else return this.name
            },
            link_avatar : function(){
                return "/assets/img/avatar/" + this.avatar
            },
            link_image : function(){
                return "/assets/img/video/" + this.image
            },
            link_src : function(){
                return this.link
            },
            link_gif : function(){
                return "/assets/img/video/" + this.gif
            }
        },
        methods : {
            hover : function(){
                this.imgshow = this.link_gif == '' ? this.link_image : this.link_gif
            },
            dishover : function(){
                this.imgshow = this.link_image
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

.col-2{
    width: 20%;
}

.col-8{
    width: 75%;
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
  width: calc(26% - 36px);
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