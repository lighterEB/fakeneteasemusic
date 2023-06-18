<template>
    <ItemMusicTop :playlist="state.playlist"/>
</template>
<script>
import { useRoute } from 'vue-router';
import { reactive,onMounted } from 'vue';
import {getMusicItemList} from '@/request/api/item.js'
import ItemMusicTop from '@/components/item/ItemMusicTop.vue'
export default {
    setup(){
        const state=reactive({
            playlist:{}
        });
        onMounted(async ()=>{
            let id = useRoute().query.id;
            let res = await getMusicItemList(id)
            console.log(res.data.playlist.description.length);
            state.playlist=res.data.playlist
        });
        // 防止页面刷新，数据丢失，将数据保存在sessionStorage中
        sessionStorage.setItem('itemDetail', JSON.stringify(state))
        return {
            state,
        }
    },
    components:{
        ItemMusicTop
    }
};
</script>