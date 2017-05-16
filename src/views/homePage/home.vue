<template>
    <x-content v-scroll-record topShow title="首页" style="padding-bottom:50px">
        我是首页{{ date | transitionDate('HH:MM:SS') }}

        <div id="main" style="width:100%;height: 300px;"></div>


        <swiper :options="swiperOption" ref="mySwiper">
          <!-- 幻灯内容 -->
          <swiper-slide v-for="(item,index) in list" :key="item.id">
            <div class="x-home-redbag-box">
              <div class="x-home-redbag" flex="main:left cross:center">
                <img src="./../../assets/images/demo/demo_logo.png" alt="" height="60%">
                <div class="x-title">
                  <h3>大世面面馆{{item.name}}</h3>
                  <p class="margin-top-5">给你发了一个红包</p>
                </div>
              </div>
            </div>
          </swiper-slide>
        </swiper>
        <p v-for="item in 50">{{userInfo.username}}</p>
        <x-button @on-click="alertShow = true">切换</x-button>
        <x-button @on-click="goNext()">跳转到下一个页面</x-button>

        <x-alert :show="alertShow" title="我是测试" @on-cancel="alertShow = false"></x-alert>
    </x-content>
</template>

<script>
    import echarts from 'echarts'
    import swiper from './../../mixins/swiper' // 引入混合的swiper
    import { mapGetters , mapActions } from 'vuex'
    export default {
        mixins: [swiper],
        data () {
            return {
                date:'1494924702',
                alertShow : false,
                list:[
                    {
                      id:1,
                      name:'111'
                    },
                    {
                        id:2,
                        name:'222'
                    }
                ]
            }
        },
        computed : {
            ...mapGetters([
                'userInfo'
            ]),
        },
        created(){
            this.getHomeDate();
        },
        mounted(){
            this.initEcharts();
        },
        methods : {
            ...mapActions([
                'uploadUserInfo'
            ]),
            goNext(){
                this.$router.push({name : 'test1'});
            },
            getHomeDate(){
                this.uploadUserInfo({
                    page : 1,
                    pageSize : 3 ,
                    recommend : 1
                }).then(res=>{
                    console.log(res);
                });
            },
            initEcharts(){
                var myChart = echarts.init(document.getElementById('main'));

                // 绘制图表
                myChart.setOption({
                    title: {
                        text: '折线图堆叠'
                    },
                    tooltip: {
                        trigger: 'axis'
                    },
                    legend: {
                        data:['邮件营销','联盟广告','视频广告','直接访问','搜索引擎']
                    },
                    grid: {
                        left: '3%',
                        right: '4%',
                        bottom: '3%',
                        containLabel: true
                    },
                    toolbox: {
                        feature: {
                            saveAsImage: {}
                        }
                    },
                    xAxis: {
                        type: 'category',
                        boundaryGap: false,
                        data: ['周一','周二','周三','周四','周五','周六','周日']
                    },
                    yAxis: {
                        type: 'value'
                    },
                    series: [
                        {
                            name:'邮件营销',
                            type:'line',
                            stack: '总量',
                            data:[120, 132, 101, 134, 90, 230, 210]
                        },
                        {
                            name:'联盟广告',
                            type:'line',
                            stack: '总量',
                            data:[220, 182, 191, 234, 290, 330, 310]
                        },
                        {
                            name:'视频广告',
                            type:'line',
                            stack: '总量',
                            data:[150, 232, 201, 154, 190, 330, 410]
                        },
                        {
                            name:'直接访问',
                            type:'line',
                            stack: '总量',
                            data:[320, 332, 301, 334, 390, 330, 320]
                        },
                        {
                            name:'搜索引擎',
                            type:'line',
                            stack: '总量',
                            data:[820, 932, 901, 934, 1290, 1330, 1320]
                        }
                    ]
                });
            }
        },
    }
</script>

<style lang="less" scoped>
    @import "./../../assets/css/modules/home.less";
</style>
