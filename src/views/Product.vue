<template>
    <div class="box">
        <el-carousel
            height="calc(100vh - 60px)"
            direction="vertical"
            motion-blur
            :autoplay="false"
        >
            <el-carousel-item v-for="item in 4" :key="item">
            <h3 text="2xl" justify="center">{{ item }}</h3>
            </el-carousel-item>
        </el-carousel>
    </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import axios from 'axios';
const product = ref([]);

onMounted(()=>{
    getProduct();
})

const getProduct = async()=>{
    const res = await axios.get('/webapi/product/getProduct');
    console.log(res);
}

</script>

<style scoped>
.box{
    margin-top: 60px;
}

.demonstration {
  color: var(--el-text-color-secondary);
}

.el-carousel__item h3 {
  color: #475669;
  opacity: 0.75;
  line-height: 200px;
  margin: 0;
  text-align: center;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
}

.el-carousel__item:nth-child(2n + 1) {
  background-color: #d3dce6;
}
</style>