<template>
    <div class="wrap">
        <div class="container">
            <div class="dw-title">
                提现&nbsp;
                <img alt="" src="../../../static/imgs/recharge.png" style="margin-top:-4px;">
            </div>
            <div class="dw-body f-cb">
                <div class="f-fl">
                    <div class="depositBox">
                        <div class="asset-info">
                            <div class="asset-content f-cb">
                                <p id="select" @click="selClick"><span ref="selBZ">选择币种</span><span class="bt_arrow"></span></p>
                                <ul class="selInfo" v-show="selList">
                                    <li @click="selText(item.code)" v-for="(item, index) of $store.getters.currency.params" :key="index">{{item.code}}</li>
                                </ul>
                            </div>
                            <div>
                                <table class="curAsset">
                                    <colgroup width="33%"></colgroup>
                                    <colgroup width="33%"></colgroup>
                                    <colgroup width="33%"></colgroup>
                                    <tr>
                                        <th>总额</th>
                                        <th>下单冻结</th>
                                        <th>可用余额</th>
                                    </tr>
                                    <br>
                                    <tr>
                                        <td>{{Number(totalMoeny).toFixed(2)}} {{unit}}</td>
                                        <td>{{Number(frozenMoney).toFixed(2)}} {{unit}}</td>
                                        <td>{{Number(totalMoeny - frozenMoney).toFixed(2)}} {{unit}}</td>
                                    </tr>
                                </table>
                                <div class="speBox">
                                    <div class="importants">
                                        <strong>注意: </strong>
                                        <p>• 最小提现数量为: {{withdrawMin}}<span>{{unit}}</span>。</p>

                                        <p>• 请勿直接提现至众筹或ICO地址.我们不会处理未来代币的发放。</p>

                                        <p>• 请务必确认电脑及浏览器安全，防止信息被篡改或泄露。</p>

                                    </div>
                                    <div class="asset-address selectBoxFull">
                                        <p id="selAddr" @click="selAddrClick"><span ref="selAdr">选择地址</span><span class="bt_arrow"></span></p>
                                        <ul class="selInfo" v-show="addrShow">
                                            <li v-for="(item, index) of addrLists" :key="index" @click="addAddr(item.currencyCode)">{{item.currencyCode}}</li>
                                        </ul>
                                    </div>
                                    <div class="withdrawal">
                                        <div class="f-cb">
                                            <strong>
                                                提现地址
                                            </strong>
                                        </div>
                                        <div class="selectBoxFull ">
                                            <input type="text" disabled placeholder="提现地址" v-model="withdrawAddr">
                                        </div>
                                    </div>
                                    <div class="withdrawal">
                                        <div class="f-cb">
                                            <strong>
                                                提现数量
                                            </strong>
                                        </div>
                                        <div class="selectBoxFull ">
                                            <input type="text" placeholder="提现数量" v-model="moneyOutNum" style="background: #fff">
                                        </div>
                                    </div>
                                    <div class="total-content f-cb">
                                        <span>手续费：<strong>{{withdrawFee}}</strong></span>
                                        <span>实际到账数量：<strong>{{moneyOutNum?(parseFloat(moneyOutNum) - parseFloat(withdrawFee)):0}}</strong></span>
                                    </div>
                                    <div class="submitBtn">
                                        <input type="button" class="btn btn-orange btn-block" value="确认" @click="confirmMO">
                                    </div>
                                    <div class="tips">
                                        <strong>提示</strong>
                                        <p>• 您可以在充值提现 <router-link :to="{path: 'chargeHistory',query: {typeNum: 1}}">历史记录</router-link> 页面跟踪状态。</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="f-fr">
                    <div class="lists deposit">
                        <div class="lists-title">
                            历史记录
                        </div>
                        <table class="lists-body" v-show="!moneyOutShow">
                            <colgroup width="20%"></colgroup>
                            <colgroup width="20%"></colgroup>
                            <colgroup width="20%"></colgroup>
                            <colgroup width="20%"></colgroup>
                            <colgroup width="20%"></colgroup>
                            <tr>
                                <th>时间</th>
                                <th>币种</th>
                                <th>数量</th>
                                <th>状态</th>
                                <th>地址</th>
                            </tr>
                            <br>
                            <tr v-for="(item, index) of moneyOutList" :key="index">
                                <td>{{item.createTime}}</td>
                                <td>{{item.currencyCode}}</td>
                                <td>{{item.number}}</td>
                                <td>{{item.status == '0'?'审核中': item.status == '1'?'成功': item.status == '2'?'失败': ''}}</td>
                                <td>{{item.address}}</td>
                            </tr>
                        </table>
                        <div class="nodata" v-show="moneyOutShow">
                            <img src="../../../static/imgs/record.png">
                            <p class="" style="margin-left:-4px;">无提现记录</p>
                        </div>
                        <div style="text-align: center;margin-top: 50px" v-show="pageShow">
                            <Page :total="Number(totalList)" :page-size="10" size="small" show-total  @on-change="togglePage"/>
                        </div>
                    </div>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
    export default {
        name: "moneyOut",
        data(){
            return {
                selList: false,
                unit: '',//单位
                moneyOutNum: '',//提现数量
                totalMoeny: '',//总共金额
                frozenMoney: '',//冻结金额
                withdrawFee: 0,//手续费
                withdrawMin: '',//最小提币额
                addrLists: [],//提现地址列表
                addrShow: false,
                withdrawAddr: '',//提现地址
                moneyOutShow: true,//是否有提现记录
                moneyOutList: [],//提现记录
                pageShow: true,
                totalList:'',//分页总条数
            }
        },
        mounted(){
            this.getQueryMoneyOut(0);//获取提现记录
        },
        methods: {
            selClick(){//点击选择币种
                this.selList = this.selList? false : true;
            },
            selText(txt){//选中某一项内容
                this.$refs.selBZ.innerHTML = txt;
                this.unit = txt;
                this.selList = false;
                this.getBZInfos();//获取币币的资产信息
                this.getServiceFee();//提取该币种的手续费
                this.getMoneyOutAddr(txt);//获取提现地址
                this.$refs.selAdr.innerHTML = '选择地址';
                this.withdrawAddr = '';
            },
            getBZInfos(){//获取币币的资产信息
                this.$axios({
                    url: '/api/assets/query',
                    method: 'post',
                    params: {
                        type: '1',
                    }
                })
                    .then(res => {
                        if(res.data.errorCode === 0){
                            if(res.data.data && res.data.data.length !== 0){
                                res.data.data.forEach((val,index) => {
                                    if(val.currencyCode == this.$refs.selBZ.innerHTML){
                                        this.totalMoeny = val.amount;
                                        this.frozenMoney = val.frozenAmount;
                                    }
                                })

                            }
                        }
                    })
                    .catch(err => {
                        this.$Message.warning(err.data.errorMsg);
                    })
            },
            getServiceFee(){//获取提现手续费
                this.$axios({
                    url: '/api/trade/currency_list',
                    method: 'post',
                })
                    .then(res => {
                        if(res.data.errorCode === 0){
                            if(res.data.data && res.data.data.length !== 0){
                                res.data.data.forEach((item,index) => {
                                    if(item.code == this.$refs.selBZ.innerHTML){
                                        this.withdrawFee = item.withdrawFee;
                                        this.withdrawMin = item.withdrawMin;
                                    }
                                })
                            }
                        }
                    })
                    .catch(err => {
                        this.$Message.warning(err.data.errorMsg);
                    })
            },
            getMoneyOutAddr(item){//获取提现地址
                this.$axios({
                    url: '/api/wallet/queryaddress',
                    method: 'post',
                    params: {
                        start: 0,
                        rows: 100,
                        currency: item || ''
                    }
                })
                    .then(res => {
                        if(res.data.errorCode === 0){
                            if(res.data.page.data.length !== 0){
                                this.addrLists = res.data.page.data;
                                res.data.page.data.forEach((val) => {
                                    if(val.currencyCode == this.$refs.selAdr.innerHTML){
                                        this.withdrawAddr = val.address;
                                    }
                                })
                            }
                        }
                    })
                    .catch(err => {
                        this.$Message.warning(err.data.errorMsg);
                    })
            },
            selAddrClick(){//点击选择地址
                this.addrShow = this.addrShow? false : true;

            },
            addAddr(txt){//点击选择地址中的某一项
                this.$refs.selAdr.innerHTML = txt;
                this.addrShow = false;
                this.getMoneyOutAddr(txt);//获取提现地址
            },
            confirmMO(){
                if(this.$refs.selBZ.innerHTML == '选择币种'){
                    this.$Message.info('请选择币种');

                }else if(this.$refs.selAdr.innerHTML == '选择地址'){
                    this.$Message.info('请选择地址');
                }else if(!this.moneyOutNum){
                    this.$Message.info('请输入提现数量');
                }else {
                    //调用提现的接口
                    this.moneyOut();
                }
            },
            moneyOut(){//提现的接口
                this.$axios({
                    url: '/api/wallet/moneyout',
                    method: 'post',
                    data:{
                        currencyCode: this.$refs.selBZ.innerHTML,
                        number: this.moneyOutNum,
                    },
                })
                    .then(res => {
                        if(res.data.errorCode === 0){
                            this.$Message.info(res.data.errorMsg);
                            this.moneyOutNum = '';
                            //调用获取提现记录的接口
                            this.getQueryMoneyOut(0);
                        }else {
                            this.$Message.warning(res.data.errorMsg);
                        }
                    })
                    .catch(err => {
                        this.$Message.warning(err.data.errorMsg);
                    })
            },
            getQueryMoneyOut(start){//获取提现记录
                this.$axios({
                    url: '/api/wallet/querymoneyout',
                    method: 'post',
                    data:{
                        start,
                        rows: 10,
                    },
                })
                    .then(res => {
                        if(res.data.errorCode === 0){
                            if(res.data.page.data && res.data.page.data.length != 0){
                                this.moneyOutList = res.data.page.data;
                                this.moneyOutShow = false;
                                this.totalList = res.data.page.total;
                                if(this.totalList > 10){
                                    this.pageShow = true;
                                }else {
                                    this.pageShow = false
                                }
                                this.totalList = res.data.page.total;
                            }else {
                                this.moneyOutShow = true;
                            }
                        }
                    })
                    .catch(err => {
                        this.$Message.warning(err.data.errorMsg);
                    })
            },
            togglePage(curPage){//点击分页事件
                this.getQueryMoneyOut((curPage-1)*10);
            }
        }
    }
</script>

<style scoped>
    .wrap {
        background: #f5f4fc;
        min-width: 1200px;
        padding-bottom: 30px;
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
        font-size: 18px;
        color: #333;
        padding: 10px 0;
        margin-bottom: 10px;
    }
    .dw-body {
        background-color: #fff;
        border-radius: 10px !important;
        margin-bottom: 80px;
    }
    .dw-body > .f-fl, .dw-body > .f-fr {
        width: 638px;
        min-height: 200px;
    }
    .f-fl {
        float: left;
    }
    .f-fr {
        float: right;
    }
    .depositBox .asset-info {
        padding: 38px 58px 20px;
    }
    .depositBox .curAsset {
        width: 100%;
        margin: 20px 0 20px;
    }
    .depositBox .curAsset td {
        padding-top: 10px;
        border-top: 1px solid #ddd;
        text-align: center;
        margin-top: 10px;
        font-size: 12px;
    }
    .depositBox .curAsset th {
        font-size: 12px;
        color: #666;
        text-align: center;
        border-left: 1px solid #ddd;
    }
    .f-cb:after, .f-cbli li:after {
        display: block;
        clear: both;
        visibility: hidden;
        height: 0;
        overflow: hidden;
        content: ".";
    }
    #select, #selAddr {
        white-space: nowrap;
        overflow: hidden;
        width: 100%;
        padding-right: 1.5em;
        border-radius: 3px;
        color: #a7b7da;
        height: 32px;
        line-height: 32px;
        font-size: 1rem;
        border: 1px solid rgb(233, 238, 248);
        cursor: pointer;
        position: relative;
    }
    #select span.bt_arrow, #selAddr span.bt_arrow {
        border: .4rem solid transparent;
        border-top-color: #a7b7da;
        cursor: pointer;
        width: 0;
        height: 0;
        position: absolute;
        right: .5rem;
        top: .8rem;
    }
    .asset-content, .asset-address {
        position: relative;
        min-width: 100%;
    }

    ul.selInfo {
        width: 100%;
        padding: .4em;
        border: 1px solid #D5D5D5;
        box-shadow: 0px 3px 5px #ddd;
        position: absolute;
        display: block;
        z-index: 1005;
        top: 32px;
        left: 0px;
        border-radius: 3px;
        opacity: 1;
        height: auto;
        background: #ffffff;
        color: #222222;
    }
    ul.selInfo li {
        line-height: 35px ;
        border: 0 none;
        cursor: pointer;
        margin: 1px 0;
        padding: .125em .25em ;
        text-align: left;
    }
    .speBox {
        background: #e9eef8;
        padding: 15px 20px;
    }
    .importants {
        font-size: 12px;
    }
    .importants strong {
        display: inline-block;
        margin-bottom: 6px;
        font-weight: bold;
    }
    .importants p {
        padding-top: 0.3rem;
        line-height: 1.5;
    }

    .dw-body > .f-fr {
        width: 538px;
    }
    .lists {
        line-height: 30px;
    }
    .lists .lists-title {
        background: #e9eef8;
        font-size: 12px;
        color: #999;
        border: none;
        margin-top: 38px;
        margin-right: 40px;
        border-radius: 4px;
    }
    .lists.deposit .lists-title, .lists.deposit .lists-body {
        padding: 0 10px;
    }
    .lists-body {
        margin-right: 40px;
    }
    .lists-body td{
        text-align: center;
        vertical-align: middle;
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
    .selectBoxFull {
        position: relative;
        width: 100%;
        padding-right: 0px;
        box-sizing: border-box;
        height: 34px;
        margin-top: 10px;
        margin-bottom: 24px;
    }
    .withdrawal input[type="text"] {
        background: #f3f3f3;
        display: inline-block;
        width: 100%;
        border: 1px solid #e6e6e6;
        box-sizing: border-box;
        padding: 0 10px;
        line-height: 32px;
        font-size: 14px;
    }
    .submitBtn .btn-block {
        font-size: 14px;
        width: 100%;
        background: #5b8fef !important;
        color: #c8d8fc!important;
        border: none;
        height: 34px;
        text-align: center;
        padding: 9px 0;
        cursor: pointer;
        font-weight: 400;
        border-radius: 0.25rem;
    }
    .btn-orange:hover {
        background: #317aff!important;
        border-color: #f1c55f!important;
        color: #fff!important;
    }
    .total-content {
        font-size: 12px;
        color: #8892b7;
        line-height: 100%;
        margin-bottom: 15px;
        text-align: center;
    }
    .total-content span {
        padding: 5px 60px;
    }
    .tips {
        margin-top: 20px;
    }
    .tips strong {
        color: #333;
        font-size: 12px;
        display: inline-block;
        margin-bottom: 6px;
        font-weight: bold;
    }
    .tips p {
        font-size: 12px;
        color: #666;
        line-height: 20px;
    }
</style>
