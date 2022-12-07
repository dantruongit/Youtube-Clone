<template>
    <div>
        <div class="formMSG">
            <div class="msg">
                Chức năng này đang xây dựng
            </div>
        </div>
    </div>
</template>
<script>

</script>
<style scoped>

@keyframes TopToMiddle{
    from {
        margin-top : 0%;
        opacity: 0;
    }
    to{
        margin-top : 10%;
        opacity: 1;
    }
}

.form{
    background-color: #f0f3f8;
    margin-top : 10%;
    width: 450px;
    height: auto;
    min-height: 300px;
    border-radius: 10px;
    display: inline-block;
    animation: TopToMiddle ease 0.3s;
}

.msg{
    padding-top: 15%;
    margin-bottom: 5%;
    font-size: 24px;
}
</style>