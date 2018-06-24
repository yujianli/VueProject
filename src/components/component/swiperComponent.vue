<template>
    <div>
        <div class="swiper-area">
            <van-swipe :autoplay="1000">
                <van-swipe-item v-for="(banner ,index) in bannerData" :key="index">
                    <img v-lazy="banner.image" alt=""  width="100%">
                </van-swipe-item>
            </van-swipe>
        </div>
    </div>
</template>

<script>
    export default {
        props:['bannerData']
    }
</script>

<style scoped>
    .swiper-area{
        clear:both;
        max-height: 15rem;
        overflow: hidden;
    }
</style>