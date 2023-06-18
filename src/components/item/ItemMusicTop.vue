<template>
    <div>
        <div class="itemMusicTop">
            <img :src="playlist.coverImgUrl" alt="" class="bgimg">
            <div class="itemLeft">
                <svg class="icon" aria-hidden="true" @click="$router.go(-1)">
                    <use xlink:href="#icon-fanhui"></use>
                </svg>
                <span>歌单</span>
            </div>
            <div class="itemRight">
                <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-sousuo"></use>
                </svg>
                <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-caidan"></use>
                </svg>
            </div>
            <div class="playlist-detail">
                <img :src="playlist.coverImgUrl" alt="playlist-img">
                <span class="playCount">
                    <svg class="icon" aria-hidden="true">
                        <use xlink:href="#icon-bofang4"></use>
                    </svg>
                    {{ changeCount(playlist.playCount) }}
                </span>
                <p class="name">{{ playlist.name }}</p>
                <div class="right_img">
                    <img :src="playlist.creator.backgroundUrl" alt="">
                    <span class="nickname">{{ playlist.creator.nickname }}</span>
                    <svg class="icon" aria-hidden="true">
                        <use xlink:href="#icon-details"></use>
                    </svg>
                    <div v-if="isDescriptionLoaded">
                        <p class="rightP_two" :class="{ 'expanded': isDescriptionExpanded }">
                            {{ truncatedDescription }}
                            <span is-link @click="showPopup">更多</span>
                            <van-popup v-model:show="show"
                                :style="{ height: '80%', right: '25px', backgroundColor: 'rgba(0, 0, 0, 0.5)', backdropFilter: 'blur(8px)', position: '{absolute}' }">
                                <img :src="playlist.creator.backgroundUrl"
                                    style="display: flex; margin-top: 10px; margin-left: 38%; width: 80px; height: 80px;" />
                                <p style="display: flex; margin: 5%">
                                    {{ playlist.description }}
                                </p>
                            </van-popup>
                        </p>
                    </div>

                </div>
            </div>

            <!-- <div class="contentRight">
                <p class="rightP_one">{{ playlist.name }}</p>
                <div class="right_img">
                    <img :src="playlist.creator.backgroundUrl" alt="">
                    <span>{{ playlist.creator.nickname }}</span>
                    <svg class="icon" aria-hidden="true">
                        <use xlink:href="#icon-caidan"></use>
                    </svg>
                </div>
            </div>
            <p class="rightP_two">
                <span>{{ playlist.description }}</span>
                <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-caidan"></use>
                </svg>
            </p>
        </div>
        <div class="itemTopFooter">
            <div class="footerItem">
                <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-caidan"></use>
                </svg>
                <span>{{ playlist.commentCount }}</span>
            </div>
            <div class="footerItem">
                <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-caidan"></use>
                </svg>
                <span>{{ playlist.shareCount }}</span>
            </div>
            <div class="footerItem">
                <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-caidan"></use>
                </svg>
                <span>下载</span>
            </div>
            <div class="footerItem">
                <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-caidan"></use>
                </svg>
                <span>多选</span>
            </div>-->
            <div class="selectMenu">
                <span>
                    <svg class="icon" aria-hidden="true">
                        <use xlink:href="#icon-pinglun1"></use>
                    </svg>
                    <span class="text">评论</span>
                </span>
                <span>
                    <svg class="icon" aria-hidden="true">
                        <use xlink:href="#icon-fenxiang4"></use>
                    </svg>
                    <span class="text">分享</span>
                </span>
                <span>
                    <svg class="icon" aria-hidden="true">
                        <use xlink:href="#icon-xiazai"></use>
                    </svg>
                    <span class="text">下载</span>
                </span>
                <span>
                    <svg class="icon" aria-hidden="true">
                        <use xlink:href="#icon-UIsheji_menjinxitong-13"></use>
                    </svg>
                    <span class="text">多选</span>
                </span>
            </div>
        </div>

    </div>
</template>
<script>
import { computed } from '@vue/reactivity'
import { onMounted, ref } from 'vue'

export default {
    setup(props) {
        // 如果props没有数据，从sessionstorage中获取
        if ((props.playlist.creator = "")) {
            props.playlist.creator = JSON.parse(sessionStorage.getItem("").playlist).creator
        };
        function changeCount(num) {
            if (num >= 100000000) {
                return (num / 100000000).toFixed(1) + "亿"
            } else if (num >= 10000) {
                return (num / 10000).toFixed(1) + "万"
            }
        };
        const show = ref(false);
        const showPopup = () => {
            show.value = true;
        };
        const description = ref(''); //歌曲描述
        const isDescriptionLoaded = ref(false); //描述是否已加载
        const isDescriptionExpanded = ref(false); //描述是否展开

        const loadDescription = () => {
            setTimeout(() => {
                const data = props.playlist.description;
                description.value = data;
                console.log(description)
                isDescriptionLoaded.value = true;
            }, 500);
        };
        // const truncatedDescription = (desc) => {
        //     console.log(desc);
        // const maxLength = 15;
        // if(desc.toString().length > maxLength) {
        //     return desc.toString().slice(0,maxLength) + "...";
        // }
        // return desc.toString();
        // };
        onMounted(() => {
            loadDescription();
        });

        // 截取描述文本
        const truncatedDescription = computed(() => {
            const maxLength = 15;
            if (description.value.length > maxLength && !isDescriptionExpanded.value) {
                return description.value.slice(0, maxLength) + '...';
            }
            return description.value;
        });

        // 描述文本是否需要被截取
        const isDescriptionTruncated = computed(() => {
            return description.value.length > 15;
        });
        // 切换描述展开/收起状态
        const toggleDescription = () => {
            showPopup.value = !showPopup.value;
            console.log(showPopup)
        };

        return {
            changeCount,
            description,
            isDescriptionLoaded,
            truncatedDescription,
            isDescriptionTruncated,
            isDescriptionExpanded,
            toggleDescription,
            show,
            showPopup,
        }
    },

    props: ['playlist']
}
</script>
<style lang="less" scoped>
.itemMusicTop {
    width: 100%;
    height: 1rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: relative;

    .itemLeft,
    .itemRight {
        width: 25%;
        height: 100%;
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin: 0 10px;

        span {
            font-size: 20px;
            color: aliceblue;
        }

        .icon {
            fill: aliceblue;
        }
    }

    .bgimg {
        width: 100%;
        height: 11rem;
        position: fixed;
        z-index: -1;
        filter: blur(0.4rem);
    }

    .playlist-detail {
        position: absolute;
        margin-top: 60%;
        margin-left: 5%;
        display: flex;

        img {
            display: block;
            width: 35%;
            height: auto;
            border-radius: 10%;
        }

        .playCount {
            position: absolute;
            align-items: center;
            display: flex;
            margin: 0 0.8rem;
            background-color: rgba(24, 1, 1, 0.5);
            color: #bceef5;
            font-size: 0.28rem;
            width: 1.6rem;
            border-radius: 0.4rem;
        }

        .playCount svg {
            width: 12px;
            height: 12px;
            margin-right: 5px;
        }

        .name {
            font-size: .28rem;
            color: azure;
            margin: 0rem 0.6rem;
        }

        .right_img {
            position: absolute;
            width: 2rem;
            height: 12px;
            margin-right: 5px;
            margin: 50px 20px 40px 140px;

            img {
                border-radius: 50%;
            }

            .icon {
                position: absolute;
                width: 30px;
                display: flex;
                top: 0.1rem;
                left: 2.8rem;
                width: 100px;
            }

            .nickname {
                position: absolute;
                display: flex;
                top: 8px;
                left: 50px;
                width: 130px;
                font-size: .28rem;
                color: azure;
            }

            .rightP_two {
                margin-top: 10%;
                width: 200px;
                color: #d1cece;
                font-size: 13px;
                overflow: hidden;
                text-overflow: ellipsis;
            }

            .expanded {
                white-space: normal;
            }

            .popup-content {
                padding: 16px;
            }
        }

    }

    .selectMenu {
        display: flex;
        position: absolute;
        margin-top: 120%;
        color: #d1c9c9;

        span {
            margin-left: 14px;

            .icon {
                width: 56px;
            }
        }

        span.text {
            backdrop-filter: blur(10px);
            background-color: rgba(10, 10, 10, 0.5);
            border-radius: 10%;
        }

    }
}
</style>