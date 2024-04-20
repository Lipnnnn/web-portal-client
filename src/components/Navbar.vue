<template>
    <div class="contain">
        <el-menu
            :default-active="route.fullPath"
            class="el-menu-demo"
            mode="horizontal"
            :router = "true"
        >
            <el-menu-item index="/">首页</el-menu-item>
            <el-menu-item index="/news">新闻中心</el-menu-item>
            <el-menu-item index="/product">产品与服务</el-menu-item>
            <el-menu-item @click="handleClick">登录</el-menu-item>
        </el-menu>
        <div class="title">Lipn公司门户网站</div>
    </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRoute } from 'vue-router';
const route = useRoute();

// 跳转到登录页面
const handleClick = ()=>{
    window.location = 'http://localhost:9999'
}

</script>

<style scoped>
.contain{
    height: 60px;
    width: 100%;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 99;
    margin-left: 20px;
}

.title{
    position: fixed;
    top: 15px;
    right: 20px;
}
</style>