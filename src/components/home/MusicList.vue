<template>
    <div class="musicList">
        <div class="musicTop">
            <div class="title">发现好歌单</div>
            <div class="more">查看更多</div>
        </div>
        <div class="musicContent">
            <van-swipe :loop="false" :width="150" class="my-swipe" :show-indicators="false">
                <van-swipe-item v-for="item in state.musicList" :key="item">
                    <router-link :to="{path:'/itemmusic', query:{id:item.id}}">
                    <img :src="item.picUrl" alt="">
                    <span class="playCount">
                        <svg class="icon" aria-hidden="true">
                            <use xlink:href="#icon-bofang4"></use>
                        </svg>
                        {{ changeCount(item.playCount) }}
                    </span>
                    <span class="name">{{ item.name }}</span>
                </router-link>
                </van-swipe-item>
            </van-swipe>
        </div>
    </div>
</template>
<script>
import { getMusicList } from '@/request/api/home.js'
import { reactive, onMounted } from 'vue';
export default {
    // -------------------vue2--------------------------
    // data() {
    //     return {
    //         musicList: []
    //     }
    // },
    // methods: {
    //     async getGedan() {
    //         let res = await getMusicList();
    //         console.log(res);
    //         this.musicList = res.data.result
    //     },
    //     changeCount: function (num) {
    //         if (num >= 100000000) {
    //             return (num / 100000000).toFixed(1) + "亿"
    //         } else if (num >= 10000) {
    //             return (num / 10000).toFixed(1) + "万"
    //         }
    //     }
    // },
    // mounted() {
    //     this.getGedan()
    // }
    // -------------------vue2--------------------------
    // 
    // -------------------vue3--------------------------
    setup() {
        const state = reactive({
            musicList: [],
        });
        onMounted(async () => {
            let res = await getMusicList();
            // console.log(res.data.result);
            state.musicList = res.data.result
        });
        function changeCount(num) {
            if (num >= 100000000) {
                return (num / 100000000).toFixed(1) + "亿"
            } else if (num >= 10000) {
                return (num / 10000).toFixed(1) + "万"
            }
        }
        return {
            state,
            changeCount
        };
    }
};
</script>
<style lang="less" scoped>
.musicList {
    width: 100%;
    height: 5rem;
    padding: 0.2rem;

    .musicTop {
        width: 100%;
        height: 0.6rem;
        display: flex;
        justify-content: space-between;
        margin-bottom: 0.2rem;

        .title {
            font-size: 0.4rem;
            font-weight: 900;
        }

        .more {
            border: 1px solid;
            text-align: center;
            line-height: 0.6rem;
            padding: 0 0.2rem;
            border-radius: 0.4rem;
        }
    }

    .musicContent {
        width: 100%;
        height: 4rem;

        img {
            height: 2.4rem;
        }

        .playCount {
            position: absolute;
            right: 10px;
            top: 44%;
            background-color: rgba(24, 1, 1, 0.5);
            color: #bceef5;
            padding: 5px;
            font-size: 14px;
            width: 1.9rem;
            border-radius: 0.4rem;
        }

        .my-swipe {
            height: 100%;
        }
        .playCount svg {
            width: 16px;
            height: 16px;
            margin-right: 5px;
        }
    }
}
</style>