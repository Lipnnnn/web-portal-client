<template>
    <div class="box">
        <el-carousel
            height="calc(100vh - 60px)"
            direction="vertical"
            motion-blur
            :autoplay="true"
        >
            <el-carousel-item v-for="item in productList" :key="item._id">
                <div class="item" :style="{backgroundImage: `url(http://localhost:3333${item.image})`}">
                    <el-card>
                        <template #header>
                        <div class="item.name">
                            <h2>{{item.name}}</h2>
                        </div>
                        </template>
                        <p style="margin-bottom: 40px;">简介：{{ item.introduction }}</p>
                        <p style="margin-bottom: 80px; line-height: 2; text-indent: 2em;">{{ item.detail }}</p>
                        <p>了解更多，请加微信：Lipn552576</p>
                    </el-card>
                </div>
            </el-carousel-item>
        </el-carousel>
    </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import axios from 'axios';
const productList = ref([]);

onMounted(()=>{
    getProduct();
})

const getProduct = async()=>{
    const res = await axios.get('/webapi/product/list');
    productList.value = res.data.data;
}

</script>

<style scoped>
.box{
    margin-top: 60px;
}
.item{
    width: 100%;
    height: 100%;
    background-size: cover;
}
.el-card{
    width: 50%;
    height: 100%;
    background-color: rgba(255,255,255,.7);
}

</style>