<template>
    <div id="Content">
        <div class="notice">
            <span>在开始交易之前，您需要完善必要的交易信息<router-link to="/personal" >去填写</router-link></span>
        </div>
        <div id="newAssets">
            <div class="ass-tab">
                <ul class="clear">
                    <li :class="{assActives : curIndex == index}" v-for="(item, index) of assetsTabs" :key="index" @click="toggleTab(index)">
                        <span>{{item}}</span>
                    </li>
                </ul>
            </div>
            <div class="ass-switch" v-show="curIndex == 0">
                <div class="ass-money clear">
                    <div class="ass-money-li" v-for="(item, index) of assetsInfos" :key="index" @click="assetsClick(index, item.assetsType)">
                        <div class="money-li-cont">
                            <span>{{item.assetsType == 0? 'C2C': item.assetsType == 1? '币币':item.assetsType == 2? '合约': ''  }}资产估值 (USDT)</span>
                            <h6>{{item.btc}}</h6>
                            <em>≈{{item.cny}} CNY</em>
                        </div>
                        <p v-show="assetsIndex == index"></p>
                    </div>
                </div>
                <p>
                    <i class="iconfont icon-start"></i>
                    <span>温馨提示:资产估值并不代表实际价值,仅供参考</span>
                </p>
                <div class="ass-list">
                    <div class="ass-list-top">
                        <div class="list-top-left">
                            <span>{{currency}}账户</span>
                        </div>
                        <div class="list-top-right">
                            <div class="list-top-hide" @change="showHide">
                                <input type="checkbox" name="box" v-model="checked">
                                <span>隐藏资产为0的币种</span>
                            </div>
                            <!--<div class="list-top-search">-->
                                <!--<input type="text">-->
                                <!--<i class="iconfont icon-sousuo">-->

                                <!--</i>-->
                            <!--</div>-->
                        </div>
                    </div>
                    <div class="ass-list-tab" >
                        <table border="0" cellspacing="0" cellpadding="0" >
                            <tr>
                                <th>币种</th>
                                <th>余额</th>
                                <th v-show="assetsIndex != 2">下单冻结</th>
                                <th>操作</th>
                            </tr>
                            <tr v-for="(item, index) of assetsCurInfos" :key="index" v-show="initShow">
                                <td>{{item.currencyCode}}</td>
                                <td>{{item.amount}}</td>

                                <td v-show="assetsIndex != 2">{{item.frozenAmount}}</td>
                                <td>
                                    <router-link to="/chargeIn" v-show="assetsIndex == 1">充币</router-link>
                                    <router-link to="/moneyOut" v-show="assetsIndex == 1">提币</router-link>
                                    <router-link to="">交易</router-link>
                                    <a href="javascript:;" @click="assetsHZ(currency,item.currencyCode,item.amount)">资产划转</a>
                                </td>
                            </tr>
                        </table>
                    </div>
                </div>
            </div>


            <div class="ass-switch" v-show="curIndex == 1">
                <div class="ass-det clear">
                    <span>资产明细</span>
                </div>
                <div class="ass-det-list">
                    <table border="0" cellspacing="0" cellpadding="0">
                        <tr>
                            <th>方向</th>
                            <th>币种</th>
                            <th>时间</th>
                            <th>数量</th>
                        </tr>
                        <tr v-for="(item, index) of transferRecord" :key="index" v-show="!moneyOutShow">
                            <td>{{item.assetsType[0]=='0'?'C2C':item.assetsType[0]=='1'?'币币':item.assetsType[0]=='2'?'合约':'无效'}} => {{item.assetsType.split(',')[1]=='0'?'C2C':item.assetsType.split(',')[1]=='1'?'币币':item.assetsType.split(',')[1]=='2'?'合约':'无效'}}</td>
                            <td>{{item.currencyCode}}</td>
                            <td>{{item.createTime}}</td>
                            <td>{{item.difMoney}}</td>
                        </tr>
                    </table>


                </div>
                <div class="nodata" v-show="moneyOutShow">
                    <img src="../../../static/imgs/record.png">
                    <p class="" style="margin-left:-4px;">无划转记录</p>
                </div>
                <div style="text-align: center;margin-bottom: 50px;padding-top: 50px " v-show="pageShow">
                    <Page ref="pages" :total="Number(totalList)" :page-size="10" size="small" show-total  @on-change="togglePage"/>
                </div>


            </div>

        </div>


        <!--点击资产划转按钮时出的弹框-->
        <div id="AssPop" v-show="assetsTransferState">
            <div class="ass-pop">
                <div class="ass-pop-cont">
                    <div class="ass-pop-head">
                        <span>资产划转</span>
                        <em>提币请划转至币币交易账户</em>
                        <i class="iconfont icon-iconfontguanbi" @click="closeTK"></i>
                    </div>
                    <div class="ass-pop-content clear">
                        <span>{{bzTypeName}}</span>
                        <div class="sel-left">
                            <select>
                                <option value="">{{accountName}}交易账户</option>
                            </select>
                            <p>{{accountName}}交易账户余额：</p>
                            <i>{{banlance}} {{bzTypeName}}</i>
                        </div>
                        <em>&gt;&gt;</em>
                        <div class="sel-left">
                            <select @change="changeTxt(curVal)" v-model="curVal">
                                <option :value="item" v-for="(item, index) of accountLists" :key="index" >{{item}}交易账户</option>
                            </select>
                            <p>{{changAccount}}交易账户余额：</p>
                            <i>{{banlanceDF}} {{bzTypeName}}</i>
                        </div>
                        <div class="ass-pop-input">
                            <input type="text" placeholder="数量" v-model="allNum" oninput="this.value=this.value.replace(/[^\d.]/g,'')">
                            <span @click="totalNumClick">全部</span>
                        </div>
                    </div>
                    <div class="ass-pop-but">
                        <button @click="closeTK">取消</button>
                        <button class="confirm" @click="confirmHZ">立即划转</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "newassets",
        data(){
            return {
                assetsTabs: ['交易账户', '划转明细'],
                curIndex: 0,//当前tab下标
                assetsIndex: 0,//当前资产的下标
                assetsInfos: [],//资产概况数据
                currency: 'C2C',//币种
                assetsCurInfos: [],//每个币种对应的数据列表信息
                transferRecord: [],//资金划转记录
                assetsTransferState: false,//点击资产划转按钮时的状态改变
                checked: '',
                initShow: true,
                totalList: '',
                pageShow: true,
                moneyOutShow: true,
                bzTypeName: '',//币种名
                accountName: '',//账户名
                accountLists: ['C2C', '合约', '币币'],
                changAccount:'',//改变后的交易账户
                curVal: '',
                banlance:'',//余额
                banlanceDF: '',//转向账户的余额
                allNum: '',
            }
        },
        mounted(){
            this.getAssetsSurveyQuery();//获取资产概况查询
            this.getAssetsQuery('0');
        },
        methods: {
            toggleTab(index){//资产中的交易账户和划转明细切换
                this.curIndex = index ;
                this.$refs.pages.currentPage = 1;

                if(index == 1){
                    this.getTransferRecord(0);//资金划转记录查询
                }
            },
            assetsClick(index, assetsType){//资产中的交易账户三个资产切换
                this.assetsIndex = index;
                this.currency = assetsType == 0?'C2C': assetsType == 1?'币币': assetsType == 2?'合约':'';

                this.getAssetsQuery(assetsType);//调用对应币种获取的数据信息
            },
            getAssetsSurveyQuery(){//获取资产概况查询
                this.$axios.post('/api/assets/survey/query')
                    .then(res => {
                        if(res.data.errorCode === 0){
                            if(res.data.data && res.data.data.length != 0){
                                this.assetsInfos = res.data.data;
                            }
                        }else {
                            this.$Message.warning(res.data.errorMsg);
                        }
                    })
                    .catch(err => {
                        this.$Message.warning(err.data.errorMsg);
                    })
            },
            getAssetsQuery(curBZType){//获取具体资产种类的列表数据
                this.$axios({
                    url: '/api/assets/query',
                    method: 'post',
                    params: {
                        type: curBZType
                    },
                })
                    .then(res => {
                        if(res.data.errorCode === 0){
                            if(res.data.data && res.data.data.length != 0){
                                this.assetsCurInfos = res.data.data;
                            }
                        }else {
                            this.$Message.warning(res.data.errorMsg);
                        }
                    })
                    .catch(err => {
                        this.$Message.warning(err.data.errorMsg);
                    })
            },
            getTransferRecord(start){//资金划转记录查询
                this.$axios({
                    url: '/api/assets/transfer/query',
                    method: 'post',
                    params: {
                        start,
                        rows: '10',
                        type: 'Z'
                    },
                })
                    .then(res => {
                        if(res.data.errorCode === 0){
                            if(res.data.page.data && res.data.page.data.length != 0){
                                this.transferRecord = res.data.page.data;
                                this.totalList = res.data.page.total;
                                if(this.totalList > 10){
                                    this.pageShow = true;
                                }else {
                                    this.pageShow = false
                                }
                                this.moneyOutShow = false;
                            }
                        }else {
                            this.$Message.warning(res.data.errorMsg);
                        }
                    })
                    .catch(err => {
                        this.$Message.warning(err.data.errorMsg);
                    })
            },
            assetsHZ(zhType,bzName,moneyBan){//点击数据列中的资产划转按钮
                this.allNum = '';
                this.accountName = zhType;
                this.bzTypeName = bzName;
                this.banlance = moneyBan;//余额
                this.accountLists = ['C2C', '合约', '币币'],
                this.accountLists.forEach((val,index) => {
                    if(zhType == val){
                        this.accountLists.splice(index,1);
                    }
                })

                this.curVal = this.accountLists[0];//select默认选中第一个
                this.changAccount = this.accountLists[0];//select默认选中第一个
                this.aginGetQueryMoney(this.curVal == 'C2C'?'0':this.curVal == '币币'?'1':this.curVal == '合约'?'2':'Z');

                this.assetsTransferState = true;
            },
            closeTK(){//关闭资产划转的弹框
                this.assetsTransferState = false;
            },
            totalNumClick(){//点击弹框中的全部
                this.allNum = this.banlance;
            },
            showHide(){//多选框的选中操作
                if(this.checked){
                    this.initShow = false;
                }else {
                    this.initShow = true;
                }
            },
            togglePage(curPage){//分页点击事件
                this.getTransferRecord((curPage-1)*10);
            },
            changeTxt(txt){//select改变事件
                this.changAccount = txt;
                let vurN = txt == 'C2C'?'0':txt == '币币'?'1':txt == '合约'?'2':'Z';
                this.aginGetQueryMoney(vurN);


            },
            aginGetQueryMoney(curT){
                this.$axios({
                    url: '/api/assets/query',
                    method: 'post',
                    params: {
                        type: curT,
                    },
                })
                    .then(res => {
                        if(res.data.errorCode === 0){
                            if(res.data.data && res.data.data.length != 0){
                                res.data.data.forEach((val, index) => {
                                    if(val.type == curT && this.bzTypeName == val.currencyCode){
                                        this.banlanceDF = val.amount;
                                    }
                                })
                            }
                        }else {
                            this.$Message.warning(res.data.errorMsg);
                        }
                    })
                    .catch(err => {
                        this.$Message.warning(err.data.errorMsg);
                    })
            },
            confirmHZ(){//点击立即划转后
                // 判断输入的转出数量不能超出自有的最大数量
                if(this.allNum){
                    // this.assetDetail();
                }

                if(this.allNum > this.banlance){
                    this.$Message.warning('输入值超出可划转数量');
                }else if(!this.allNum){
                    this.$Message.warning('请输入大于0的数量');
                }else {
                    this.getTransfer();
                }
            },
            getTransfer() {//资金划转的接口
                console.log(111)
                let rawAddress =  this.accountName.charCodeAt(0) == 'C2C'.charCodeAt(0)? 0 :
                                  this.accountName.charCodeAt(0) == '币币'.charCodeAt(0)? 1 :
                                  this.accountName.charCodeAt(0) == '合约'.charCodeAt(0)? 2 : -1;

                let targetAddress = this.changAccount.charCodeAt(0) == 'C2C'.charCodeAt(0)? 0 :
                                    this.changAccount.charCodeAt(0) == '币币'.charCodeAt(0)? 1 :
                                    this.changAccount.charCodeAt(0) == '合约'.charCodeAt(0)? 2 : -1;
                this.$axios({
                    url: '/api/assets/transfer',
                    method: 'post',
                    data: {
                        rawAddress,
                        targetAddress,
                        exchangeCode: this.bzTypeName,
                        amount: this.allNum,
                    }
                })
                    .then(res => {
                        if(res.data.errorCode === 0){
                            this.$Message.info(res.data.errorMsg);
                            this.allNum = '';
                            this.assetsTransferState = false;

                            this.getAssetsQuery(rawAddress);


                        }else {
                            this.$Message.warning(res.data.errorMsg);
                        }
                    })
                    .catch(err => {
                        this.$Message.warning(err.data.errorMsg);
                    })

            }
        },
        components: {}

    }
</script>

<style scoped>
    #Content {
        min-height: calc(100% - 578px);
    }
    .notice {
        width: 1200px;
        margin: 0 auto;
        font-size: 14px;
        padding-top: 20px;
    }
    .notice>span {
        display: block;
        background: #fff2ce;
        padding: 10px 20px;
    }
    .notice>span>a {
        padding-left: 10px;
        color: #648cd4;
    }
    #newAssets {
        width: 1200px;
        margin: 0 auto;
    }
    .ass-tab {
        border-bottom: 2px solid #eee;
        padding-top: 30px;
        text-align: center;
    }
    .ass-tab>ul {
        display: inline-block;
    }
    .clear:after {
        content: "";
        clear: both;
        display: block;
    }
    .ass-tab>ul>li {
        float: left;
        padding: 0 42px;
        height: 40px;
    }
    .ass-tab>ul>li>span {
        line-height: 40px;
        font-size: 17px;
        padding: 0 6px;
        color: #000;
        display: inline-block;
        cursor: pointer;
    }
    .assActives>span {
        color: #0c88f9!important;
        border-bottom: 2px solid #0c88f9;
    }
    .ass-money {
        padding: 25px 0 20px;
    }
    .ass-money-li {
        float: left;
        width: 33.33%;
        box-sizing: border-box;
        padding: 0 30px;
        text-align: center;
        color: #fff;
    }
    .money-li-cont {
        height: 154px;
        cursor: pointer;
    }
    .ass-money-li:first-child .money-li-cont {
        background: url(../../../static/imgs/c2c.png) no-repeat 0 0;
        background-size: 100% 100%;
    }
    .ass-money-li:nth-child(2) .money-li-cont {
        background: url(../../../static/imgs/bb.png) no-repeat 0 0;
        background-size: 100% 100%;
    }
    .ass-money-li:nth-child(3) .money-li-cont {
        background: url(../../../static/imgs/heyue.png) no-repeat 0 0;
        background-size: 100% 100%;
    }
    .money-li-cont>span {
        font-size: 17px;
        line-height: 70px;
    }
    .money-li-cont>h6 {
        font-size: 21px;
        padding-bottom: 20px;
    }
    .money-li-cont>em {
        font-size: 14px;
    }
    .ass-money-li p {
        width: 96%;
        height: 4px;
        border-radius: 4px;
        margin: 6px auto;
    }
    .ass-money-li:first-child p {
        background: #83e1ce;
    }
    .ass-money-li:nth-child(2) p {
        background: #82bff9;
    }
    .ass-money-li:nth-child(3) p {
        background: #b3b1f9;
    }
    .ass-switch>p {
        font-size: 13px;
        padding-left: 62px;
        color: #666;
    }
    .ass-switch>p>i {
        font-size: 15px;
    }
    .ass-list {
        padding-top: 34px;
    }
    .ass-list-top {
        height: 52px;
        background: #eaeef9;
        line-height: 52px;
        font-size: 14px;
    }
    .list-top-left {
        float: left;
        width: 50%;
    }
    .list-top-left>span {
        padding-left: 62px;
    }
    .list-top-right {
        float: left;
        width: 50%;
        text-align: right;
        padding-right: 46px;
        box-sizing: border-box;
    }
    .list-top-hide {
        display: inline-block;
        padding-right: 40px;
    }
    .list-top-search {
        display: inline-block;
        position: relative;
    }
    .list-top-search>input {
        width: 160px;
        height: 22px;
        border-radius: 4px;
        border: 1px solid #9db4d8;
        padding-right: 28px;
        outline: none;
        background: #eaeef9;
    }
    .list-top-search>i {
        font-size: 16px;
        color: #5b719e;
        position: absolute;
        top: 0;
        right: 6px;
        cursor: pointer;
    }
    .ass-list-tab {
        min-height: 560px;
        background: #fff;
        margin-bottom: 50px;
        padding: 30px 24px;
    }
    .ass-list-tab>table {
        width: 100%;
    }
    .ass-list-tab>table td {
        text-align: center;
        vertical-align: middle;
    }
    .ass-list-tab>table td, .ass-list-tab>table th {
        height: 40px;
        font-size: 15px;
        line-height: 40px;
    }
    .ass-list-tab>table td:first-child, .ass-list-tab>table th:first-child {
        text-align: left;
        padding-left: 38px;
    }
    .ass-list-tab>table tr td:last-child, .ass-list-tab>table tr th:last-child {
        text-align: right;
        padding-right: 30px;
        width: 30%;
    }
    .ass-list-tab>table td>a {
        color: #6583f9;
        font-size: 15px;
        padding-left: 36px;
    }

    .ass-det {
        height: 88px;
    }
    .ass-det>span {
        font-size: 15px;
        padding: 0 40px;
        float: left;
        line-height: 88px;
    }
    .ass-det-list {
        background: #fff;
        padding: 0 35px;
        min-height: 400px;
    }
    .ass-det-list>table {
        width: 100%;
        margin: 20px 0;
    }
    .ass-det-list>table th {
        font-size: 16px;
        height: 56px;
        line-height: 56px;
        border-bottom: 1px solid #e9e9e9;
    }
    .ass-det-list > table td {
        text-align: center;
        vertical-align: middle;
        padding: 15px 0;
    }
    .ass-det-list>table td:first-child, .ass-det-list>table td:nth-child(2), .ass-det-list>table th:first-child, .ass-det-list>table th:nth-child(2) {
        width: 15%;
    }
    .ass-det-list>table td:nth-child(3), .ass-det-list>table th:nth-child(3) {
        padding-right: 64px;
        width: 20%;
    }
    .ass-det-list>table td:nth-child(4), .ass-det-list>table th:nth-child(4) {
        text-align: right;
        padding-right: 64px;
        width: 30%;
    }
    .ass-det-list>table td:last-child, .ass-det-list>table th:last-child {
        text-align: right;
        padding-right: 64px;
        width: 20%;
    }
    .ass-page {
        margin: auto;
        margin-bottom: 50px;
        background: #fff;
        padding-bottom: 30px;
        text-align: center;
    }
    #AssPop {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0,0,0,.6);
    }
    .ass-pop {
        width: 670px;
        margin: 0 auto;
        padding-top: 100px;
    }
    .ass-pop-cont {
        padding: 38px;
        background: #fff;
        border-radius: 6px;
    }
    .ass-pop-head {
        height: 64px;
        border-bottom: 1px solid #eee;
        position: relative;
    }
    .ass-pop-head>span {
        font-size: 21px;
    }
    .ass-pop-head>em {
        font-size: 15px;
        padding-left: 40px;
    }
    .ass-pop-head>i {
        position: absolute;
        top: -15px;
        right: 0;
        font-size: 18px;
        cursor: pointer;
    }
    .ass-pop-but {
        text-align: center;
        padding-top: 20px;
    }
    .ass-pop-but>button {
        width: 105px;
        height: 36px;
        border: 1px solid #c6c6c6;
        background: #fff;
        outline: none;
        cursor: pointer;
        border-radius: 6px;
        margin-right: 130px;
    }
    .ass-pop-but>button:last-child {
        margin: 0;
    }
    .ass-pop-content {
        padding: 40px 0;
    }
    .ass-pop-content>span {
        float: left;
        font-size: 17px;
        line-height: 30px;
        padding-right: 8px;
    }
    .sel-left {
        float: left;
        font-size: 14px;
        line-height: 20px;
    }
    .sel-left>i, .sel-left>p {
        font-size: 12px!important;
    }
    .sel-left>select {
        width: 160px;
        height: 32px;
        border: 1px solid #cdcdcd;
        border-radius: 4px;
        outline: none;
        margin-bottom: 5px;
    }
    .ass-pop-content>em {
        float: left;
        padding: 0 5px;
        font-size: 17px;
        line-height: 30px;
    }
    .ass-pop-input {
        float: right;
        width: 166px;
        height: 32px;
        border: 1px solid #cdcdcd;
        box-sizing: border-box;
        border-radius: 4px;
        background: #f6f6f6;
        overflow: hidden;
    }
    .ass-pop-input>input {
        width: 64%;
        background: #fff;
        height: 30px;
        border: 0;
        float: left;
        outline: none;
        text-align: center;
    }
    .ass-pop-input>span {
        float: left;
        width: 36%;
        display: inline-block;
        background: #f6f6f6;
        color: #529beb;
        line-height: 30px;
        text-align: center;
        font-size: 17px;
        cursor: pointer;
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
