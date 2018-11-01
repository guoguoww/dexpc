<template>
    <div class="wrap">
        <div class="container">
            <div class="dw-title">
                充值&nbsp;
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
                                        <p>• 请勿向上述地址充值任何非  资产，否则资产将不可找回。</p>

                                        <p>• 您充值至上述地址后，需要整个网络节点的确认，在此过程中请耐心等待。</p>

                                        <p>• 最小充值金额：{{minRechargMoney}} ，小于最小金额的充值将不会上账且无法退回。</p>

                                        <p>• 您的充值地址不会经常改变，可以重复充值；如有更改，我们会尽量通过网站公告或邮件通知您。</p>
                                    </div>

                                    <div>
                                        <div class="depositAddress">
                                            <div class="chargeAddress text">充值地址：<span>{{imgAddrUrl}}</span></div>

                                            <div class="f-cb">
                                                <span class="btn btn-white btn-copy " @click="copyAddr" id="copyBtn" style="margin:10px 70px;">
                                                  复制到剪切板
                                                </span>
                                                <span class="btn btn-white" @click="showEWM">展示二维码</span>
                                            </div>
                                            <p style="margin: 10px 0">• 转账后请查看交易ID，如实填写下方信息，以确保快速到账。</p>
                                            <div class="selectBoxFull" style="margin-bottom:24px;">
                                                <input class="selectIpt"  placeholder="充值数量" v-model="reNum" type="text">
                                            </div>

                                            <div class="submitBtn">
                                                <input type="button" value="确认" class="btn-orange btn-block" @click="confirmR">
                                            </div>


                                            <div class="tips">
                                                <strong class="">提示</strong>
                                                <span style="font-size:12px;">充值完成后，你可以进入 <router-link class="yellow" :to="{path: 'chargeHistory',query: {typeNum: 0}}">历史记录</router-link> 页面跟踪进度</span>
                                                <ul class="ng-scope">
                                                    <li class="" style="display: block;">

                                                    </li>

                                                </ul>
                                            </div>
                                        </div>
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
                        <table class="lists-body" v-show="!chargeShow">
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
                            <tr v-for="(item, index) of chargeList" :key="index">
                                <td>{{item.createTime}}</td>
                                <td>{{item.currencyCode}}</td>
                                <td>{{item.number}}</td>
                                <td>{{item.status == '0'?'审核中': item.status == '1'?'成功': item.status == '2'?'失败': ''}}</td>
                                <td>{{item.address}}</td>
                            </tr>
                        </table>
                        <div class="nodata" v-show="chargeShow">
                            <img src="../../../static/imgs/record.png">
                            <p  style="margin-left:-4px;">无充值记录</p>
                        </div>
                        <div style="text-align: center;margin-top: 50px" v-show="pageShow">
                            <Page :total="Number(totalList)" :page-size="10" size="small" show-total  @on-change="togglePage"/>
                        </div>
                    </div>
                </div>
            </div>
        </div>


        <div class="ui-widget-overlay ui-dialog-mask" style="z-index: 1060;" v-show="isShow">
            <div class="ui-dialog-titlebar">
                <p>钱包地址二维码 <span class="iconfont icon-iconfontguanbi" @click="closeBtn"></span></p>
                <div class="ui-dialog-content">
                    <div >
                        <img :src="ewmUrl" alt="">
                    </div>
                </div>
            </div>
            <div>

            </div>
        </div>

    </div>
</template>

<script>
    export default {
        name: "chargeIn",
        data(){
          return {
              selList: false,
              unit: '',//单位
              totalMoeny: '',//总共金额
              frozenMoney: '',//冻结金额
              chargeList: [],//充值记录
              chargeShow: true,
              minRechargMoney: '',
              imgAddrUrl: '',//充值地址
              ewmUrl: '',//二维码路径
              isShow: false,
              reNum: '',//充值数量
              totalList: '',
              pageShow: true,
          }
        },
        mounted(){
            this.getRechargeRecord(0);//充值记录
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
                this.getRechargeMinMoney();//获取最小充值金额
                this.getQueryQrcode();//查询充值二维码
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
            getQueryQrcode(){//查询充值二维码
                this.imgAddrUrl = '';
                this.$axios({
                    url: '/api/wallet/query/qr_code',
                    method: 'post',
                    data:{
                        currencyCode: this.$refs.selBZ.innerHTML,
                    },
                })
                    .then(res => {
                        if(res.data.errorCode === 0){
                            //查询成功后返回的数据再调用生成图片的接口
                            if(res.data.data && res.data.data.length !== 0){
                                console.log(1111)
                                this.imgAddrUrl = res.data.data.address;
                                this.getImage(res.data.data.qrCodeImage);
                            }

                        }else {
                            this.$Message.warning(res.data.errorMsg);
                        }
                    })
                    .catch(err => {
                        this.$Message.warning(err.data.errorMsg);
                    })
            },
            getRechargeMinMoney(){//获取最小充值金额
                this.$axios({
                    url: '/api/trade/currency_list',
                    method: 'post',
                })
                    .then(res => {
                        if(res.data.errorCode === 0){
                            if(res.data.data && res.data.data.length !== 0){
                                res.data.data.forEach((item,index) => {
                                    if(item.code == this.$refs.selBZ.innerHTML){
                                        this.minRechargMoney = item.rechargeMin;
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
            getRechargeRecord(start){//获取充值记录
                this.$axios({
                    url: '/api/wallet/queryrecharge',
                    method: 'post',
                    data:{
                        start,
                        rows: 10,
                    },

                })
                    .then(res => {
                        if(res.data.errorCode === 0 ){
                            if(res.data.page.data && res.data.page.data.length != 0){
                                this.chargeList = res.data.page.data;
                                this.chargeShow = false;
                                this.totalList = res.data.page.total;
                                if(this.totalList > 10){
                                    this.pageShow = true;
                                }else {
                                    this.pageShow = false
                                }
                            }else {
                                this.chargeShow = true;
                            }
                        }
                    })
                    .catch(err => {
                        this.$Message.warning(err.data.errorMsg);
                    })
            },
            getImage(imgId){//获取图片的base64的接口
                this.$axios.get('/api/image/'+ imgId)
                    .then(res => {
                        if(res.data){
                            this.ewmUrl = res.data;
                        }
                    })
                    .catch(err => {
                    })
            },
            copyAddr(){//点击复制地址
                if(this.$refs.selBZ.innerHTML != '选择币种'){
                    let oInput = document.createElement('input');
                    oInput.value = this.imgAddrUrl;
                    document.body.appendChild(oInput);
                    oInput.select(); // 选择对象
                    document.execCommand("Copy"); // 执行浏览器复制命令
                    oInput.className = 'oInput';
                    oInput.style.display='none';

                    this.$Message.info('复制成功');
                }else {
                    this.$Message.info('充值地址为空!')
                }

            },
            closeBtn(){//关闭生成二维码的弹框
                this.isShow = false;
            },
            showEWM(){
                if(this.$refs.selBZ.innerHTML != '选择币种'){
                    this.isShow = true;
                }else {
                    this.$Message.info('钱包地址为空，请先选择相应的币种!')
                }

            },
            getRecharge(){//充值的接口
                this.$axios({
                    url: '/api/wallet/recharge',
                    method: 'post',
                    params:{
                        currencyCode: this.$refs.selBZ.innerHTML,
                        number: this.reNum,
                    },
                })
                    .then(res => {
                        if(res.data.errorCode === 0){
                            this.$Message.info(res.data.errorMsg);
                            this.reNum = '';
                            //调用充值记录的接口
                            this.getRechargeRecord(0);
                        }else {
                            this.$Message.warning(res.data.errorMsg);
                        }
                    })
                    .catch(err => {
                        this.$Message.warning(err.data.errorMsg);
                    })
            },
            confirmR(){//确认充值按钮
                if(this.$refs.selBZ.innerHTML == '选择币种') {
                    this.$Message.info('请选择币种');
                }else if(!this.imgAddrUrl){
                    this.$Message.info('充值地址为空');
                }else if(!this.reNum) {
                    this.$Message.info('请选择充值数量');
                }else {//调用充值的接口
                    this.getRecharge();
                }
            },
            togglePage(curPage){//点击分页事件
                this.getRechargeRecord((curPage-1)*10);
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
    #select {
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
    #select span.bt_arrow {
        border: .4rem solid transparent;
        border-top-color: #a7b7da;
        cursor: pointer;
        width: 0;
        height: 0;
        position: absolute;
        right: .5rem;
        top: .8rem;
    }
    .asset-content {
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
    .depositAddress {
        margin-top: 15px;
    }
    .depositAddress .chargeAddress {
        display: inline-block;
        width: 100%;
        border-sizing: border-box;
        border: 1px solid #e6e6e6;
        font-weight: bold;
        font-size: 14px;
        color: #333;
        text-align: center;
        background: #f8f8f8;
        margin: 6px 0;
        line-height: 32px;
        padding: 5px 10px;
    }
    .depositAddress .chargeAddress.text {
        box-sizing: border-box;
        word-break: break-all;
        text-align: left;
        padding: 6px 24px;
        line-height: 18px;
        color: #666;
    }
    .selectBoxFull {
        position: relative;
        width: 100%;
        padding-right: 0px;
        box-sizing: border-box;
        height: 34px;
    }
    .selectBoxFull .selectIpt {
        background: #fff;
        display: inline-block;
        width: 100%;
        height: 100%;
        border: 1px solid #e6e6e6;
        box-sizing: border-box;
        padding: 0 10px;
        line-height: 32px;
        font-size: 14px;
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
    .depositAddress .btn {
        background: #fff;
        font-size: 12px;
    }
    .btn {
        display: inline-block;
        cursor: pointer;
        font-size: 14px;
        color: #666;
        padding: 9px 12px;
        border: 1px solid #d4d4d4;
        line-height: 100%;
        background: #f5f5f5;
        font-weight: 400;
        text-align: center;
        white-space: nowrap;
        vertical-align: middle;
        border-radius: 0.25rem;
    }
    .depositAddress .btn:hover {
        background: #e8b342!important;
        border-color: #e8b342!important;
        color: #fff;
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
    .ui-dialog-mask {
        position: fixed;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        background: rgba(102,102,102,.5);
    }
    .ui-dialog-titlebar {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
        width: 350px;
        height: 350px;
        background: #fff;
        padding: 1em 1.5em;
        border-radius: 3px;
    }
    .ui-dialog-titlebar p {
        padding: 1em 1.5em;
        font-size: 1.25em;
    }
    .ui-dialog-titlebar p span{
        float: right;
        padding: .125em;
        cursor: pointer;
        border: 1px solid transparent;
        color: #1b1d1f;
        width: 1.28em;
        text-align: center;
    }
    .ui-dialog-content {
        padding: 1em 1.5em;
    }
    img {
        max-width: 100%;
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
</style>
