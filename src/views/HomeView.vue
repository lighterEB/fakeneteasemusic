<template>
  <TopNav/>
  <SwipierTop/>
  <IconList/>
  <MusicList/>
</template>

<script>
// @ is an alias to /src
import TopNav from '@/components/home/TopNav.vue';
import SwipierTop from '@/components/home/SwipierTop.vue';
import IconList from '@/components/home/IconList.vue';
import MusicList from '@/components/home/MusicList.vue';

export default {
  name: 'HomeView',
  components: {
    TopNav,
    SwipierTop,
    IconList,
    MusicList,
}
}
</script>
