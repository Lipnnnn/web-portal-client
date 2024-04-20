<template>
    <div class="news">
        <div class="search">
            <input type="text" class="input" v-model="query" @blur="handleBlur" placeholder="请输入新闻标题关键字">
            <div class="show" v-show="isShow">
                <h3>搜索结果：</h3>
                <ul>
                    <li v-for="item in queryList"><a href="">{{ item.title }}</a></li>
                </ul>
            </div>
        </div>
        <div class="list">
            <el-row :gutter="20">
                <el-col v-for="item in showNews" :span="6">
                    <el-card style="max-width: 480px" shadow="hover">
                        <template #header>{{item.title}}</template>
                        <div class="image" :style="{backgroundImage: `url(http://localhost:3333${item.cover})`}"></div>
                    </el-card>
                </el-col>
            </el-row>
        </div>
    </div>
</template>

<script setup>
import { computed, onMounted, reactive, ref, watch } from 'vue';
import axios from 'axios';

const query = ref("");
const newsList = ref([]);
const isShow = ref(false);

watch(query,()=>{
    isShow.value = true;
})

onMounted(()=>{
    getNewsList();
})

// 获取所有新闻
const getNewsList = async()=>{
    const res = await axios.get('/webapi/news/list');
    newsList.value = res.data.data;
}

// 查到的数据（计算属性）
const queryList = computed(()=>{
    if(!query.value.trim()){
        isShow.value = false;
        return [];
    }
    return newsList.value.filter(item=>item.title.includes(query.value));
})

// 失去焦点就隐藏搜索到的数据
const handleBlur = ()=>{
    isShow.value = false;
}

// 新闻列表展示，只展示前4条数据
const showNews = computed(()=>{
    return newsList.value.slice(0,4);
})

</script>

<style scoped>
.news{
    margin-top: 60px;
}
.search{
    width: 100%;
    height: 500px;
    background-image: url('@/assets/newsbg.png');
    background-size: cover;
    background-repeat: no-repeat;
    background-position-y: -150px;
    position: relative;
    z-index: 99;
}
.input{
    height: 40px;
    width: 500px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,0);
    outline: none;
    font-size: 18px;
    padding: 0 40px;
    border-radius: 10px;
    border: 1px solid white;
    background-image: url('@/assets/search.png');
    background-size: 20px,20px;
    background-position: 10px,5px;
    background-repeat: no-repeat;
}
.input::placeholder{
    font-size: 13px;
}
.show{
    width: 560px;
    overflow: auto;
    /* height: 300px; */
    background-color: #f6f5f5;
    position: absolute;
    left: 460px;
    top: 295px;
    /* box-shadow: 0 0 5px 5px rgb(235, 233, 233); */
    padding: 0 10px;
    box-sizing: border-box;
    border-radius: 10px;
}
.image{
    width: 100%;
    height: 250px;
    background-size: cover;
}
.list{
    margin: 20px;
}

</style>