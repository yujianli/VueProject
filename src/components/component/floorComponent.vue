<template>
    <div>
        <div class="floor">
            <div class="floor-title">{{floorTitle}}</div>
            <div class="floor-anomaly">
                <div class="floor-one">
                    <img :src="floorData0.image" alt="" width="100%">
                </div>
                <div>
                    <div class="floor-two">
                        <img :src="floorData1.image" alt="" width="100%">
                    </div>
                        <div>
                        <img :src="floorData2.image" alt="" width="100%">
                    </div>
                </div>
            </div>
            <div class="floor-rule">
                <div v-for="(item,index) in floorData.slice(3)" :key="index">
                    <img :src="item.image" alt="" width="100%">
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props:['floorData','floorTitle'],
        data() {
            return {
                floorData0:{},
                floorData1:{},
                floorData2:{}
            }
        },
        watch:{
            floorData:function(val) {
                this.floorData0=this.floorData[0]
                this.floorData1=this.floorData[1]
                this.floorData2=this.floorData[2]
            }
        }
    }
</script>

<style scoped>
    .floor-title{
        text-align: center;
        font-size: 14px;
        height: 1.8rem;
        line-height: 1.8rem;
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
</style>
