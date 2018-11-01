<template>
    <div class="wrap">
        <div class="container">
            <div class="dw-title">
                历史记录&nbsp;
                <img alt="" src="../../../static/imgs/time.png" style="margin-top:-4px;">
            </div>
            <div class="dw-body">
                <div class="lists-type">
                    <span v-for="(item, index) of titleInfo" :key="index" @click="toggleTab(index)" :class="{cur: getTypeNum == index}">{{item}}</span>
                </div>
                <div class="lists" v-show="!moneyOutShow">
                    <ul class="lists-title">
                        <li class="clear">
                            <div class="date">时间</div>
                            <div class="name">币种</div>
                            <div class="amount">数量</div>
                            <div class="status">状态</div>
                            <div class="address">地址</div>
                        </li>
                        <li class="clear detailInfos" v-for="(item, index) of historyInfos" :key="index">
                            <div class="date">{{item.createTime}}</div>
                            <div class="name">{{item.currencyCode}}</div>
                            <div class="amount">{{item.number }}</div>
                            <div class="status">{{item.status=='0'?'审核中':item.status=='1'?'成功':item.status=='2'?'失败':'无效状态'}}</div>
                            <div class="address">{{item.address}}</div>
                        </li>
                    </ul>
                </div>
                <div class="nodata" v-show="moneyOutShow">
                    <img src="../../../static/imgs/record.png">
                    <p class="" style="margin-left:-4px;">无历史记录</p>
                </div>
                <div style="text-align: center;margin-top: 50px" v-show="pageShow">
                    <Page ref="pages" :total="Number(totalList)" :page-size="10" size="small" show-total  @on-change="togglePage"/>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "chargeHistory",
        data(){
            return {
                titleInfo: ['充值', '提现'],
                moneyOutShow: true,
                historyInfos: [],//历史记录数据
                getTypeNum: '',
                pageShow: true,
                totalList: '',
            }
        },
        mounted(){
            this.getTypeNum = this.$route.query.typeNum;

            this.getHistoryRecord(0,this.$route.query.typeNum);//获取历史记录
        },
        methods: {
            getHistoryRecord(start,typeN){//获取历史记录
                this.$axios({
                    url: '/api/wallet/querycurrencychange',
                    method: 'post',
                    data: {
                        start,
                        rows: 10,
                        type: typeN,
                    }
                })
                    .then(res => {
                        if(res.data.errorCode === 0){
                            if(res.data.page.data && res.data.page.data.length != 0){
                                this.historyInfos = res.data.page.data;
                                this.moneyOutShow = false;
                                this.totalList = res.data.page.total;
                                if(this.totalList > 10){
                                    this.pageShow = true;
                                }else {
                                    this.pageShow = false
                                }
                            }
                        }else {
                            this.$Message.warning(res.data.errorMsg);
                        }
                    })
                    .catch(err => {
                        this.$Message.warning(err.data.errorMsg);
                    })
            },
            toggleTab(index){//切换充值提现tab
                this.getTypeNum = index;
                this.getHistoryRecord(0,index);

                //让当前页面的分页重置为第一页
                this.$refs.pages.currentPage = 1;
            },
            togglePage(curPage){
                this.getHistoryRecord((curPage-1)*10, this.getTypeNum);
            }
        }
    }
</script>

<style scoped>
    .wrap {
        background: #f5f4fc;
        min-width: 1200px;
    }
    .container {
        width: 1200px;
        margin: 0 auto;
        padding: 0;
    }
    .dw-title {
        width: 100%;
        height: 40px;
        line-height: 40px;
        font-size: 20px;
        color: #333;
        padding: 20px 0;
    }
    .dw-body {
        background-color: #fff;
        margin-top: 30px;
        border-radius: 4px;
        margin-bottom: 50px;
    }
    .lists-type {
        overflow: hidden;
        height: 50px;
        background-color: #e9eef8;
        border-top-left-radius: 4px;
        border-top-right-radius: 4px;
    }
    .lists-type span {
        display: inline-block;
        height: 50px;
        box-sizing: border-box;
        line-height: 50px;
        padding: 0 50px;
        font-size: 14px;
        color: #666;
        font-weight: 600;
        margin-left: -1px;
        cursor: pointer;
    }
    .lists-type span.cur {
        background: #fff;
        color: #74a2fe;
        border-radius: 4px;
    }
    .lists {
        background: #fff;
    }
    .lists-title li {
        color: #999;
        padding: 0;
        line-height: 30px;
    }
    .lists-title li > div {
        float: left;
        border-right: 1px solid #e6e6e6;
        padding: 0 12px;
        color: #999;
        text-align: center;
        margin-top: 10px;
        width: 200px;
    }
    .detailInfos > div {
        border-right: none !important;
    }

    .lists-title li > div.date {
        width: 240px;
    }
    .lists-title li > div.address {
        border-right: none;
    }
    .clear:after {
        content: '';
        clear: both;
        display: block;
        visibility: hidden;
        height: 0;
        overflow: hidden;
    }
    .nodata {
        text-align: center;
        padding: 20px;
    }
    .nodata img {
        width: 60px;
    }
    .nodata p {
        font-size: 14px;
        color: #999;
        margin: 10px 0 0 10px;
        line-height: 100%;
    }
</style>
