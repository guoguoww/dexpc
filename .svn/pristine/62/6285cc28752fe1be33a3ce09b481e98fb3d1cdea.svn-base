<template>
    <div class="wrap">
        <div class="container">
            <div class="dw-title">
                我的订单&nbsp;
                <!--<img alt="" src="../../../static/imgs/time.png" style="margin-top:-4px;">-->
            </div>
            <div class="dw-body">
                <div class="lists">
                    <ul class="lists-title">
                        <li class="clear">
                            <div class="orderCode">订单号</div>
                            <div class="orderType">交易类型</div>
                            <div class="orderNum">交易数量</div>
                            <div class="tradTotal">总价</div>
                            <div class="unitPrice">单价</div>
                            <div class="date">时间</div>
                            <div class="status">状态</div>
                            <div class="tradeObj">交易对象</div>
                            <div class="refNum">参考号</div>
                        </li>
                        <li class="clear detailInfo" v-for="(item, index) of tradeHistory" :key="index" v-show="!moneyOutShow">
                            <div class="orderCode">{{item.serialNo}}</div>
                            <div class="orderType">{{item.currency}}</div>
                            <div class="orderNum">{{item.orderQuantity}}</div>
                            <div class="tradTotal">{{item.orderPrice}}</div>
                            <div class="unitPrice">{{item.unitPrice}}</div>
                            <div class="date">{{item.createTime}}</div>
                            <div class="status">{{item.status=='0'?'未付款':item.status=='1'?'已付款':item.status=='2'?'已完成':item.status=='3'?'已取消':'无效状态' }}</div>
                            <div class="tradeObj">{{item.nickName}}</div>
                            <div class="refNum">{{item.id}}</div>
                        </li>
                    </ul>
                </div>
                <div class="nodata" v-show="moneyOutShow">
                    <img src="../../../static/imgs/record.png">
                    <p class="" style="margin-left:-4px;">无订单记录</p>
                </div>
            </div>
        </div>
        <div style="text-align: center;margin-bottom: 50px;padding-bottom: 20px" v-show="pageShow">
            <Page :total="Number(totalList)" :page-size="10" size="small" show-total  @on-change="togglePage"/>
        </div>
    </div>
</template>

<script>
    export default {
        name: "c2cOrder",
        data(){
            return {
                tradeHistory: [],//交易记录
                moneyOutShow: true,
                totalList: '',//总条数
                pageShow: false,//是否显示分页
            }
        },
        mounted(){
            this.getC2cOrderQuery(0);//获取c2c的全部订单
        },
        methods: {
            getC2cOrderQuery(start){//获取c2c的全部订单
                this.$axios({
                    url: '/api/trade/c2c/order_query',
                    method: 'post',
                    params: {
                        start,
                        rows: 10,
                        status: 'Z'
                    }
                })
                    .then(res => {
                        if(res.data.errorCode === 0){
                            if(res.data.page.data && res.data.page.data.length != 0){
                                this.tradeHistory = res.data.page.data;
                                this.totalList = res.data.page.total;
                                if(this.totalList > 10){
                                    this.pageShow = true;
                                }else {
                                    this.pageShow = false
                                }
                                this.moneyOutShow = false;
                            }
                        }else {
                            this.$Message.warning(res.data.errorMsg)
                        }
                    })
                    .catch(err => {
                        this.$Message.warning(err.data.errorMsg);
                    })
            },
            togglePage(pageNum){//切换分页时的点击事件
                this.getC2cOrderQuery((pageNum-1)*10);
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
        margin-bottom: 50px;
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
        width: 120px;
        text-align: center;
        margin-top: 10px;
    }
    .lists-title li > div.orderCode {
        width: 240px !important;
    }
    .detailInfo > div {
        border-right: none !important;
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
