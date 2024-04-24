<template>
    <div class="box">
        <h2>{{ currentNews.title }}</h2>
        <p>{{ currentNews.editTime }}</p>
        <el-divider>
            <el-icon><star-filled /></el-icon>
        </el-divider>
        <div v-html="currentNews.content"></div>
        <el-card style="width: 300px" class="card">
            <template #header>
            <div class="card-header">
                <h3>最近新闻</h3>
            </div>
            </template>
            <div v-for="item in latestNews" class="text item">
                <el-card>
                    <p @click="handleClick(item._id)" style="cursor: pointer;">{{ item.title }}</p>
                    <p>{{ format(item.editTime,'yyyy年MM月dd日 HH:mm') }}</p>
                </el-card>
            </div>
        </el-card>
    </div>
</template>

<script setup>
import { format } from 'date-fns';
import { StarFilled } from '@element-plus/icons-vue'
import axios from 'axios';
import { onMounted, onUpdated, reactive, ref, watchEffect } from 'vue';
import { useRouter,useRoute } from 'vue-router';
const router = useRouter();
const route = useRoute();

const currentNews = reactive({});
const latestNews = ref([]);
const limit = ref(4);

// 使用watchEffect钩子，可以监听当currentId改变的时候，就执行这个钩子里面的代码
watchEffect(async()=>{
    let currentId = route.params.id;
    const res = await axios.get(`/webapi/news/getNewsById/${currentId}`);
    Object.assign(currentNews,res.data.data);
    const res2 = await axios.get(`/webapi/news/getNewsLimit/${limit.value}`);
    latestNews.value = res2.data.data;
})

// onMounted钩子，只有在组件构建的时候才会执行，更新的时候不执行
// onMounted(async()=>{
//     let currentId = route.params.id;
//     const res = await axios.get(`/webapi/news/getNewsById/${currentId}`);
//     Object.assign(currentNews,res.data.data);
//     const res2 = await axios.get(`/webapi/news/getNewsLimit/${limit.value}`);
//     latestNews.value = res2.data.data;
// })

// onUpdated钩子，只有在组件更新的时候才会执行，
// onUpdated(async()=>{
//     let currentId = route.params.id;
//     const res = await axios.get(`/webapi/news/getNewsById/${currentId}`);
//     Object.assign(currentNews,res.data.data);
//     const res2 = await axios.get(`/webapi/news/getNewsLimit/${limit.value}`);
//     latestNews.value = res2.data.data;
// })

// 点击跳转到某个新闻的详情页
const handleClick = (id)=>{
    router.push(`/newsdetail/${id}`);
}
</script>

<style scoped>
.box{
    margin-top: 60px;
    padding: 20px 30px;
    position: relative;
}
.item{
    margin-bottom: 10px;
}
.card{
    position: absolute;
    top: 20px;
    right: 50px;
}

</style>