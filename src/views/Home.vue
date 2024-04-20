<template>
    <div>
        <el-carousel height="calc(100vh - 60px)">
            <el-carousel-item :key="1">
                <div class="home1"></div>
            </el-carousel-item>
            <el-carousel-item :key="2">
                <div class="home2"></div>
            </el-carousel-item>
            <el-carousel-item :key="3">
                <div class="home3"></div>
            </el-carousel-item>
        </el-carousel>
    </div>
</template>

<script setup>

</script>

<style scoped>
.el-carousel{
    margin-top: 60px;
}
.el-carousel div{
    width: 100%;
    height: 100%;
    background-size: cover;
}
.home1{
    background-image: url('@/assets/home1.jpg');
}
.home2{
    background-image: url('@/assets/home2.jpg');
}
.home3{
    background-image: url('@/assets/home3.jpg');
}
</style>