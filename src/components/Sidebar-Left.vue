<template>
<div class="sidebar">
    <div class="home" v-bind:class="[{active : selected === 0 }]" @click="change(0)">
        <i class="ti-home"></i>
        <p>Home</p>
    </div>
    <div class="short" v-bind:class="[{active : selected === 1}]" @click="change(1)">
        <i class="ti-video-clapper"></i>
        <p>Shorts</p>
    </div>
    <div class="sub" v-bind:class="[{active : selected === 2}]" @click="change(2)">
        <i class="ti-user"></i>
        <p>Subscriptions</p>
    </div>
    <div class="library" v-bind:class="[{active : selected === 99}]" @click="change(99)">
        <i class="ti-gallery"></i>
        <p>Library</p>
    </div>
</div>
</template>

<script>
    export default{
        name : 'Sidebar_Left',
        data() {
            return {
                selected : 0
            }
        },
        props: {
            idSelect : Number
        },
        created : function(){
            this.selected = this.idSelect
        },
        methods : {
            change : function(number){
                this.selected = number
                // if(this.selected > 3) this.selected = 0
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

div{
    color : white;
}

.home:hover, .short:hover, .sub:hover, .library:hover{
    background-color: rgba(255, 255, 255, 0.1);
    border-radius: 10px;
    cursor: pointer;
}

i, p{
    color : inherit;
}
.active{
    color : red !important;
}

.home,
.short,
.sub, .library{
    padding : 16px 0; 
    width: 100%;
    user-select: none;
}

.home i,.short i,.sub i, .library i{
    font-size: 20px;
}

.home p,.short p,.sub p, .library p{
    margin-top : 8px;
    font-size: 9px;
}


</style>