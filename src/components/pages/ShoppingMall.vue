<template>
    <div>
        <!-- S searchBar -->
        <van-row class="search-bar">
            <van-col span="3">
                <img :src="locationIcon" width="80%" alt="" class="location-icon">
            </van-col>
            <van-col span="16">
                <input type="text" class="search-input">
            </van-col>
            <van-col span="5">
                <van-button size="mini">查找</van-button>
            </van-col>
        </van-row>
        <!-- E searchBar -->

        <!-- S swiper  -->
        <swiper-component :bannerData="bannerPicArray"></swiper-component>
        <!-- E swiper  -->

        <!-- S category -->
        <div class="type-bar">
            <div v-for="(cate,index) in category" :key="index">
                <img v-lazy="cate.image" alt="" width="90%">
                <span>{{cate.mallCategoryName}}</span>
            </div>
        </div>
        <!-- E category -->

        <!-- S ads -->
        <div>
            <img v-lazy="adBanner" alt="" width="100%;">
        </div>
        <!-- E ads -->

        <!-- S recommend goods area  -->
        <div class="recommend-area">
            <div class="recommend-title">商品推荐</div>
            <div class="recommend-body">
                <swiper :options="swiperOption">
                    <swiper-slide  v-for="(item,index) in recommendGoods" :key="index">
                        <div class="recommend-item">
                            <img :src="item.image" alt="" width="80%">
                            <div>{{item.goodsName}}</div>
                            <div>￥{{item.price | moneyFilter}}(￥{{item.mallPrice | moneyFilter}})</div>
                        </div>
                    </swiper-slide>
                </swiper>
            </div>
        </div>
        <!-- E recommend goods area  -->

        <!-- S swiper test -->
        <!-- <swiper-default></swiper-default>
        <swiper-default2></swiper-default2>
        <swiper-default3></swiper-default3>
        <swiper-text></swiper-text> -->
        <!-- E swiper test -->

        <!-- S floors -->
        <floor-component :floorData="floor1" :floorTitle="floorName.floor1"></floor-component>
        <floor-component :floorData="floor2" :floorTitle="floorName.floor2"></floor-component>
        <floor-component :floorData="floor3" :floorTitle="floorName.floor3"></floor-component>
        <!-- E floors -->

        <!--Hot Area-->
        <div class="hot-area">
            <div class="hot-title">热卖商品</div>
            <div class="hot-goods">
            <!--这里需要一个list组件-->
            <van-list>
                <van-row gutter="20">
                    <van-col span="12" v-for="(item,index) in hotGoods" :key="index">
                        <goods-info :goodsImage="item.image" :goodsName="item.name" :goodsPrice="item.price"></goods-info>
                    </van-col>
                </van-row>
            </van-list>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    import 'swiper/dist/css/swiper.css'
    import {swiper,swiperSlide} from 'vue-awesome-swiper'
    import floorComponent from '../component/floorComponent'
    import swiperComponent from '../component/swiperComponent'
    import {toMoney} from '@/filter/moneyFilter.js'
    import goodsInfo from '../component/goodsInfoComponent'
    import url from '@/serviceAPI.config.js'
    // import swiperDefault from '../swiper/swiperDefault'
    // import swiperDefault2 from '../swiper/swiperDefault2'
    // import swiperDefault3 from '../swiper/swiperDefault3'
    // import swiperText from '../swiper/swiperText'
    export default {
        data() {
            return {
                swiperOption:{
                    slidesPerView:3
                },
                msg: 'shopping mall',
                locationIcon: require('../../assets/images/location.png'),
                bannerPicArray:[],
                category:[],
                adBanner:'',
                recommendGoods:[],
                floor1:[],
                floor2:[],
                floor3:[],
                floorName:{},
                hotGoods:[]
            }
        },
        filters:{
            moneyFilter(money){
                return toMoney(money)
            }
        },
        components:{
            swiper,
            swiperSlide,
            floorComponent,
            swiperComponent,
            goodsInfo
            // swiperDefault,
            // swiperDefault2,
            // swiperDefault3,
            // swiperText
        },
        created() {
            axios({
                url:url.getShoppingMallInfo,
                method: 'get'
            }).then(response=>{
                console.log(response)
                if(response.status==200) {
                    var mydata=response.data.data;
                    this.category=mydata.category;
                    this.adBanner=mydata.advertesPicture.PICTURE_ADDRESS;
                    this.bannerPicArray =mydata.slides
                    this.recommendGoods=mydata.recommend
                    this.floor1=mydata.floor1
                    this.floor2=mydata.floor2
                    this.floor3=mydata.floor3
                    this.floorName= mydata.floorName
                    this.hotGoods=mydata.hotGoods
                }
            }).catch(error=>{
                console.log('error')
            })
        }
    }
</script>

<style scoped>
    .search-bar{
        height: 2.2rem;
        background-color: #e5017d;
        line-height: 2em;
        overflow: hidden;
    }
    .search-input{
        width: 100%;
        height: 1.3rem;
        border:0;
        border-bottom: 1px solid #fff;
        background-color: #e5017d;
        color: #fff;
    }
    .location-icon{
        padding-left: 0.3rem;
        padding-top:0.2rem;
    }
    .type-bar{
        background-color: #fff;
        margin: 0 .3rem .3rem .3rem;
        border-radius: .3rem;
        font-size: 14px;
        display: flex;
        flex-direction: row;
        flex-wrap: nowrap;
    }
    .type-bar div{
        padding: .3rem;
        font-size: 12px;
        text-align: center;
    }
    .recommend-area{
        background-color: #fff;
        margin-top: .3rem;
    }
    .recommend-title{
        border-bottom: 1px solid #eee;
        font-size: 14px;
        color: #e5017d;
        padding:.2rem;
    }
    .recommend-body{
        border-bottom: 1px solid #eee;
    }
    .recommend-item{
        width:99%;
        border-right: 1px solid #eee;
        font-size: 12px;
        text-align: center;
    }
    .floor-anomaly{
        display: flex;
        flex-direction: row;
        background-color: #fff;
        border-bottom: 1px solid #ddd;
    }
    .floor-anomaly div{
        width:10rem;
        box-sizing: border-box;
        -webkit-box-sizing: border-box;
    }
    .floor-one{
        border-right: 1px solid #ddd;
    }
    .floor-two{
        border-bottom: 1px soldi #ddd;
    }
    .floor-rule{
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        background-color: #fff;
    }
    .floor-rule div{
        box-sizing: border-box;
        -webkit-box-sizing: border-box;
        flex:1;
    }
    .floor-rule div:nth-child(odd){
        border-right: 1px solid #ddd;
    }
    .hot-area{
      text-align: center;
      font-size:14px;
      height: 1.8rem;
      line-height:1.8rem;
    }
    .van-row{
        margin-left: 0 !important;
        margin-right: 0 !important;
    }

</style>
