<template>
    <div class="news">
        <div class="search">
            <input type="text" class="input" v-model="query" placeholder="请输入新闻标题关键字">
            <div class="show" v-show="isShow">
                <h3>搜索结果：</h3>
                <ul>
                    <li v-for="item in queryList" @click="handleToDetail(item._id)"><a href="">{{ item.title }}</a></li>
                </ul>
            </div>
        </div>
        <div class="list">
            <el-row :gutter="20">
                <el-col v-for="item in showNews" :span="6">
                    <el-card style="max-width: 480px" shadow="hover" @click="handleToDetail(item._id)">
                        <template #header>{{item.title}}</template>
                        <div class="image" :style="{backgroundImage: `url(http://localhost:3333${item.cover})`}"></div>
                    </el-card>
                </el-col>
            </el-row>
        </div>
        <div class="tabs">
            <el-tabs v-model="activeName" class="demo-tabs" @tab-click="handleClick">
                <el-tab-pane label="最新动态" name="1">
                    <el-row :gutter="20">
                        <el-col :span="18">
                            <el-card class="tabs-card" shadow="hover" v-for="item in news1" @click="handleToDetail(item._id)">
                                <div class="tabs-image" :style="{backgroundImage: `url(http://localhost:3333${item.cover})`}">
                                    <h3>{{ item.title }}</h3>
                                    <p>{{ formatDate(item.editTime) }}</p>
                                </div>
                            </el-card>
                        </el-col>
                        <el-col :span="6">
                            <el-timeline style="max-width: 600px">
                                <el-timeline-item
                                v-for="item in news1"
                                :timestamp="formatDate(item.editTime)"
                                >
                                {{ item.title }}
                                </el-timeline-item>
                            </el-timeline>
                        </el-col>
                    </el-row>
                </el-tab-pane>
                <el-tab-pane label="典型案例" name="2">
                    <el-row :gutter="20">
                        <el-col :span="18">
                            <el-card class="tabs-card" shadow="hover" v-for="item in news2" @click="handleToDetail(item._id)">
                                <div class="tabs-image" :style="{backgroundImage: `url(http://localhost:3333${item.cover})`}">
                                    <h3>{{ item.title }}</h3>
                                    <p>{{ formatDate(item.editTime) }}</p>
                                </div>
                            </el-card>
                        </el-col>
                        <el-col :span="6">
                            <el-timeline style="max-width: 600px">
                                <el-timeline-item
                                v-for="item in news2"
                                :timestamp="formatDate(item.editTime)"
                                >
                                {{ item.title }}
                                </el-timeline-item>
                            </el-timeline>
                        </el-col>
                    </el-row>
                </el-tab-pane>
                <el-tab-pane label="通知公告" name="3">
                    <el-row :gutter="20">
                        <el-col :span="18">
                            <el-card class="tabs-card" shadow="hover" v-for="item in news3" @click="handleToDetail(item._id)">
                                <div class="tabs-image" :style="{backgroundImage: `url(http://localhost:3333${item.cover})`}">
                                    <h3>{{ item.title }}</h3>
                                    <p>{{ formatDate(item.editTime) }}</p>
                                </div>
                            </el-card>
                        </el-col>
                        <el-col :span="6">
                            <el-timeline style="max-width: 600px">
                                <el-timeline-item
                                v-for="item in news3"
                                :timestamp="formatDate(item.editTime)"
                                >
                                {{ item.title }}
                                </el-timeline-item>
                            </el-timeline>
                        </el-col>
                    </el-row>
                </el-tab-pane>
            </el-tabs>
        </div>
    </div>
</template>

<script setup>
import { computed, onMounted, reactive, ref, watch } from 'vue';
import axios from 'axios';
import { format } from 'date-fns';
import { useRouter } from 'vue-router';
const router = useRouter();

const query = ref("");
const newsList = ref([]);
const isShow = ref(false);

watch(query,()=>{
    isShow.value = true;
})

onMounted(()=>{
    getNewsList();
    getNewsByCategory(1);
    getNewsByCategory(2);
    getNewsByCategory(3);
})

// 不同分类的新闻数据
const news1 = ref([]);
const news2 = ref([]);
const news3 = ref([]);
// 根据分类获取新闻列表
const getNewsByCategory = async(category)=>{
    const res = await axios.get(`/webapi/news/list/${category}`);
    if(category == 1){
        news1.value = res.data.data;
    }
    if(category == 2){
        news2.value = res.data.data;
    }
    if(category == 3){
        news3.value = res.data.data;
    }
}

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
});

// 选项卡部分：
const activeName = ref("1");

// 格式化日期的函数
const formatDate = (time)=>{
    return format(time,'yyyy年MM月dd日 HH:mm');
}

// 跳转到新闻详情页面
const handleToDetail = (id)=>{
    router.push(`/newsdetail/${id}`);
}


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

.tabs{
    margin: 20px;
    margin-top: 50px;
}

.tabs-card{
    margin-bottom: 20px;
}
.tabs-image{
    background-size: 100px,500px;
    background-repeat: no-repeat;
    padding-left: 120px;
}


</style>