<template>
    <div class="">
        <div id="Index">
            <div class="tab clearfix">
                <div class="tab-but">
                    <span :class="{'tab-active':tabactive==1}" @click="tabactive=1">我要购买</span>
                    <span :class="{'tab-active':tabactive==0}" @click="tabactive=0">我要出售</span>
                </div>
                <div class="tab-right">
                    <a href="javascript:;" @click="releaseAdv">发布广告</a>
                    <span @click="msgpopup=!msgpopup">
                        <i class="iconfont icon-xiaoxitongzhi"></i>
                        <em v-if='unreadmsg'>{{unreadmsg}}</em>
                    </span>
                </div>
            </div>
            <div class="index">
                <div class="index-conts clearfix" style="transform: translateX(0px);">
                    <div class="index-tab-list">
                        <div class="price">
                            <div class="price-cont clearfix">
                                <!-- <span>{{tabactive?'我要购买':'我要出售'}}</span> -->
                                <ul class="clearfix">
                                    <li v-for="(i,index) in currency" :key="index" @click="currencyitem=i;c2cdata.currency=i.code" :class="{'tabactive':c2cdata.currency==i.code}">
                                        {{i.code}}(参考价:￥
                                        <em class="index-red">{{Number(i.price).toFixed(3)}}</em>)
                                    </li>
                                </ul>
                            </div>
                        </div>
                        <div class="index-table">
                            <table border="0" cellspacing="0" cellpadding="0">
                                <tr>
                                    <th>用户昵称</th>
                                    <th>
                                        <span>成交单数</span>
                                    </th>
                                    <th>数量</th>
                                    <th class="tab-th">
                                        <span>{{payactive.name}}</span>
                                        <i class="iconfont icon-xialasanjiao"></i>
                                        <div class="tab-list">
                                            <ul>
                                                <li v-for="(i,index) in paylist" :key='index' @click="changepaytype(i)">{{i.name}}</li>
                                            </ul>
                                        </div>
                                    </th>
                                    <th>币种</th>
                                    <th>限额</th>
                                    <th>
                                        <span>单价</span>
                                    </th>
                                    <th class="tab-th">
                                        <span>货币类型</span>
                                        <i class="iconfont icon-xialasanjiao"></i>
                                        <div class="tab-list">
                                            <ul>
                                                <li>全部</li>
                                                <li>CNY</li>
                                            </ul>
                                        </div>
                                    </th>
                                    <th>操作</th>
                                </tr>
                                <tr v-for="(i,index) in c2clist" :key="index">
                                    <td>
                                        <em class="td-portrait">{{headstr(i.nickName)}}
                                            <i class="not-online"></i>
                                        </em>
                                        <span class="td-name">{{i.nickName}}
                                            <b>
                                                <!-- <time>大约2天前离线</time> -->
                                            </b>
                                        </span>
                                    </td>
                                    <td>
                                        <span>{{i.volume}}</span>
                                    </td>
                                    <td>
                                        <span>{{i.price}}</span>
                                    </td>
                                    <td>
                                        <img src="../../../static/imgs/ali.png" v-if="i.aliPay">
                                        <img src="../../../static/imgs/wx.png" v-if="i.weChatPay">
                                        <img src="../../../static/imgs/bank.png" v-if="i.bankPay">
                                    </td>
                                    <td>
                                        <span class="td-currency " :class="currencyitem.code">{{currencyitem.code}}</span>
                                    </td>
                                    <td>{{i.minQuota}}-{{i.maxQuota}}CNY
                                    </td>
                                    <td>{{i.price}}</td>
                                    <td>CNY</td>
                                    <td>
                                        <input type="button" value="购买" v-if="tabactive==1" @click="ordershow(i)">
                                        <input class="damage" type="button" value="出售" v-else @click="ordershow(i)">
                                    </td>
                                </tr>
                            </table>
                        </div>
                        <!-- 翻页 -->
                        <div id="page" style="">
                            <!-- <ul class="pagination">
                                <li class="disabled">
                                    <a href="javascript:;" class="iconfont icon-arrow-copy"></a>
                                </li>
                                <li class="active">
                                    <a href="javascript:;"> 1 </a>
                                </li>
                                <li class="">
                                    <a href="javascript:;"> 2 </a>
                                </li>
                                <li class="">
                                    <a href="javascript:;" class="iconfont icon-arrow"></a>
                                </li>
                            </ul> -->
                            <Page :total="c2ctotal" size="small" :page-size='c2cdata.rows' @on-change='pagechange' />
                        </div>
                    </div>

                    <div class="index-tab-list">
                        <div class="quick-list clearfix quick-cen"> </div>
                    </div>
                </div>
            </div>
            <!---->
            <!---->
        </div>

        <!-- 下单弹出页面 -->
        <div id="IndexPop" v-if="popup">
            <div class="index-pop">
                <div class="pop-cont">
                    <div class="pop-head">
                        <span>{{c2citem.direction==1?'购买':'出售'}} {{currencyitem.code}}</span>
                        <em>请在45秒内确定，确保以当前价格下单</em>
                        <i class="iconfont icon-iconfontguanbi" @click="popup=false"></i>
                    </div>
                    <div class="pop-content">
                        <ul>
                            <li>
                                <span>委托人: </span>
                                <em>{{c2citem.nickName}}</em>
                            </li>
                            <li>
                                <span>可支付方式：</span>
                                <img src="../../../static/imgs/ali.png" v-if="c2citem.aliPay">
                                <img src="../../../static/imgs/wx.png" v-if="c2citem.weChatPay">
                                <img src="../../../static/imgs/bank.png" v-if="c2citem.bankPay">
                            </li>
                            <li>
                                <div class="pop-back">
                                    <span>限额（CNY）</span>
                                    <em>{{c2citem.minQuota}}-{{c2citem.maxQuota}}</em>
                                </div>
                            </li>
                            <li>
                                <div class="pop-back">
                                    <span>价格（CNY）</span>
                                    <em>{{c2citem.price}}</em>
                                </div>
                            </li>
                            <li>
                                <span>数量（USDT）</span> <input type="number" step="any" min="0" placeholder="请输入数量" v-model="ordernum" @input="ordernumchange">
                            </li>
                            <li>
                                <span>金额（CNY）</span> <input type="number" step="any" min="0" placeholder="请输入金额" v-model="ordermoney" @input="ordermoneychange">
                            </li>
                            <li v-if="!c2cdata.direction">
                                <!-- margin-left: 50%;transform: translateX(-50%); -->
                                <Select v-model="selected" multiple style="width:100%;" placeholder="请选择收款方式">
                                    <Option v-for="item in options" :value="item.value" :key="item.value">{{ item.label }}</Option>
                                </Select>
                            </li>
                            <!---->
                        </ul>
                        <div class="pop-but">
                            <button @click="popup = false">{{btntext}}</button>
                            <button class="confirm" @click="order">确认</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <!-- 未读消息 -->
        <div class="news-prpmpt  " :class="{msgpopup}">
            <div class="news-header">
                <span>全部系统通知</span>
                <i class="iconfont icon-iconfontguanbi" @click="msgpopup=false"></i>
            </div>
            <div class="news-content">
                <ul>
                    <li v-for="(i,index) in msglist" :key="index" @click="skiporder(i)">
                        <a>
                            <i :class="{'news-act':!i.readStatus}"></i>
                            <!---->
                            <span>{{i.title}}</span>
                            <time>{{i.createTime}}</time>
                        </a>
                    </li>
                </ul>
                <p class="nothings">暂无未读消息</p>
            </div>
            <div class="news-footer">
                <span @click="readall">全部标记已读</span>
            </div>
        </div>
    </div>
</template>

<script type="">
export default {
    name: "c2c",
    data() {
        return {
            currency: [], //货比列表
            currencyitem: {}, //当前选择的货比
            c2cdata: {
                //获取广告列表的
                start: 0,
                rows: 10,
                direction: 1, //方向 1 买入 0 卖出
                currency: "", //币种
                payType: "" //支付方式
            },
            c2ctotal: 0, //c2c广告列表总数
            c2clist: [], //c2c广告列表
            c2citem: {}, //下单时选择的c2c广告
            tabactive: 1, //tab选项 0 买入 1 卖出
            ordermoney: 0, //交易金额
            ordernum: 0, //交易数量
            popup: false, //下单显示
            paylist: [
                //付款方式列表
                { name: "全部", type: "" },
                { name: "银行卡", type: "2" },
                { name: "支付宝", type: "0" },
                { name: "微信", type: "1" }
            ],
            payactive: { name: "全部", type: "" }, //当前付款方式
            popuptimer: null, //交易定时器
            btntext: "45秒取消", //按钮文字
            unreadmsg: 0, //未读消息
            msgpopup: false, //消息弹出
            msglist: [], //消息列表
            msgtotal: 0, //消息总数
            options: [
                //select选项
                // {id: '1', label: '支付宝'},
                // {id: '2', label: '微信'},
                // {id: '3', label: '银行卡'},
            ],
            selected: [] //select选中
        };
    },
    components: {},
    created() {
        this.getcurrency(); //获取货币
        this.getunreadmsg();
        this.getmsg();
    },
    watch: {
        tabactive(v) {
            //监测买入还是卖出
            this.c2clist = [];
            this.c2cdata.start = 0;
            this.getc2clist();
            this.c2cdata.direction = v;
        },
        "c2cdata.currency"() {
            //监测币种
            // this.dropdown=false
            this.c2clist = [];
            this.getc2clist();
        }
    },
    methods: {
        //获取货比列表
        getcurrency() {
            this.$axios
                .post("api/trade/currency_list")
                .then(res => {
                    if (!res.data.errorCode) {
                        this.currency = res.data.data; //货比列表
                        this.currencyitem = res.data.data[0]; //默认选择第一个货比
                        this.c2cdata.currency = this.currencyitem.code; //获取广告
                    }
                })
                .catch(err => {});
        },
        //获取c2c广告列表
        getc2clist() {
            if (this.c2cdata.payType == "") {
                //空字符串会被认定为 0 赋值为undefined为传空
                this.c2cdata.payType = undefined;
            }
            this.$axios({
                url: "api/trade/c2c/advertisement_list",
                method: "post",
                data: this.c2cdata
            })
                .then(res => {
                    if (!res.data.errorCode) {
                        // this.mloading=false //隐藏加载动画
                        this.c2clist = [...this.c2clist, ...res.data.page.data]; //es6合并数组
                        this.c2ctotal = res.data.page.total; //获取总数
                        // this.loading=false
                        // if (!res.data.page.data.length) {
                        //     this.loaded()
                        // }
                    }
                })
                .catch(err => {
                    console.log(err);
                });
        },
        //弹出下单
        ordershow(i) {
            if (!this.$cookie.get("_auth")) {
                this.$router.push("/login");
                return;
            }
            this.getPayList();
            this.ordermoney = 0;
            this.ordernum = 0;
            this.countdown();
            this.c2citem = i;
            console.log(i);
            this.popup = true;
        },
        //倒计时
        countdown() {
            console.log(this.popuptimer);
            clearInterval(this.popuptimer);
            this.popuptimer = null;
            var text = this.btntext;
            var num = 45;
            // this.codedisable = true;
            this.btntext = `${num--}秒取消`;
            this.popuptimer = setInterval(() => {
                this.btntext = `${num--}秒取消`;
                if (num < 0) {
                    this.btntext = text;
                    this.popup = false;
                    clearInterval(this.popuptimer);
                }
            }, 1000);
        },
        // 交易数量改变
        ordernumchange() {
            var num = "" + this.ordernum;
            var result = num.substring(0, num.indexOf(".") + 7);
            var str = num.split(".");
            if (
                str.length == 2 &&
                str[1] != undefined &&
                str[1] != "" &&
                str[1].length >= 6
            ) {
                this.ordernum = parseFloat(result);
            }
            //计算金额四舍五入保留两位小数
            this.ordermoney = (this.ordernum * this.c2citem.price).toFixed(2);
        },
        //交易金额改变
        ordermoneychange() {
            var charge = "" + this.ordermoney;
            var result = charge.substring(0, charge.indexOf(".") + 3);
            var str = charge.split(".");
            if (
                str.length == 2 &&
                str[1] != undefined &&
                str[1] != "" &&
                str[1].length >= 2
            ) {
                this.ordermoney = parseFloat(result);
            }
            this.ordernum = this.ordermoney / this.c2citem.price;
            var num = this.ordernum + "";
            num = num.substring(0, num.indexOf(".") + 7);
            this.ordernum = parseFloat(num);
        },
        //获取头像字符串
        headstr(str = "--") {
            return str.substr(0, 1);
        },
        //切换付款方式
        changepaytype(i) {
            console.log();
            this.payactive = i;
            this.c2cdata.start = 0;
            this.c2cdata.payType = i.type;
            this.c2clist = [];
            this.getc2clist();
        },
        //下单
        order() {
            if (!this.ordermoney || !this.ordernum) {
                //this.$toast('订单小于最小限额')
                this.$Notice.error({
                    title: "订单小于最小限额!"
                });
            }
            var payType = "";
            if (!this.c2cdata.direction) {
                // 判断是否为0卖出
                payType = [];
                this.selected.forEach((val, index) => {
                    console.log(val);
                    payType.push(val);
                });
                console.log(payType.join(","));
                payType = payType.join(",");
            }
            this.$axios
                .post("api/trade/c2c/place_order", {
                    advertisementId: this.c2citem.id,
                    money: parseInt(this.ordermoney),
                    payType
                })
                .then(res => {
                    console.log(res);
                    if (!res.data.errorCode) {
                        // this.$toast('下单成功!')
                        this.$Notice.open({
                            title: "下单成功!",
                            desc: "请在订单页中查看详情 "
                        });
                        this.$router.push(`/order/${res.data.data}`);
                    }
                })
                .catch(err => {});
        },
        //获取未读消息
        getunreadmsg() {
            if (this.$cookie.get("_auth")) {
                this.$axios
                    .post("api/user/user_unread_message_total")
                    .then(res => {
                        if (!res.data.errorCode) {
                            this.unreadmsg = res.data.data;
                        }
                    })
                    .catch(err => {});
            }
        },
        //获取消息列表
        getmsg() {
            if (!this.$cookie.get("_auth")) {
                return;
            }
            this.$axios
                .post("api/user/query_user_message", {
                    start: 0,
                    rows: 100
                })
                .then(res => {
                    if (!res.data.errorCode) {
                        // this.mloading=false
                        this.msglist = [...this.msglist, ...res.data.page.data];
                        this.msgtotal = res.data.page.total;
                        // this.loading = false;
                        // if (!res.data.page.data.length) {
                        //     this.loaded();
                        // }
                    }
                })
                .catch(err => {});
        },
        // 全部已读
        readall() {
            var arr = [];
            this.msglist.forEach(function(i, index) {
                if (i.readStatus == 0) {
                    console.log(i);
                    arr.push(i.id);
                }
            });
            if (!arr.join(",").length) {
                return;
            }
            this.$axios
                .get(`api/user/read_message?messageIds=${arr.join(",")}`)
                .then(res => {
                    if (!res.data.errorCode) {
                        // this.start = 0;
                        this.msglist = [];
                        this.getmsg();
                    }
                })
                .catch(err => {});
        },
        //跳转订单页
        skiporder(i) {
            if (i.readStatus == 0) {
                this.$axios
                    .get(`api/user/read_message?messageIds=${i.id}`)
                    .then(res => {
                        if (!res.data.errorCode) {
                            // this.start = 0;
                            this.msglist = [];
                            this.getmsg();
                        }
                    })
                    .catch(err => {});
            }
            this.$router.push(`/order/${i.content}`);
        },
        //获取支付列表
        getPayList() {
            this.options=[]
            this.$axios({
                url: "/api/user/paymentreceived",
                method: "post",
                params: {
                    start: 0,
                    rows: 100
                }
            }).then(res => {
                if (res.data.errorCode === 0) {
                    for (var i = 0; i < res.data.page.data.length; i++) {
                        switch (res.data.page.data[i].type) {
                            case "0":
                                res.data.page.data[i].type = "支付宝";
                                break;
                            case "1":
                                res.data.page.data[i].type = "微信";
                                break;
                            case "2":
                                res.data.page.data[i].type = "银行卡";
                                break;
                        }
                        this.options.push({
                            value: res.data.page.data[i].id,
                            label:
                                res.data.page.data[i].type +
                                ":" +
                                res.data.page.data[i].account
                        });
                    }
                }
            });
        },
        //翻页
        pagechange(index){
            console.log(index);
            this.c2clist=[]
            this.c2cdata.start=(index-1)* this.c2cdata.rows
            this.getc2clist()
            
        },
        //跳转发布广告检测
        releaseAdv() {
            if (!this.$cookie.get('_auth')) {
                this.$router.push('/login')
                return 
            }
            this.$axios.post("/api/user/query_active_user").then(res => {
                if(!res.data.errorCode) {
                    // if(res.data.data.authStatusAdvanced !== '2') {
                    //     this.$Message.warning('请先进行高级认证');
                    //     return;
                    // }
                    if(res.data.data.applyTraderStatus !== '2') {
                        this.$Message.warning('请先申请交易员');
                        return;
                    }
                    this.$router.push('/releaseAdvert')
                }
          })
            
            
        },
    }
};
</script>

<style scoped lang="less">
* {
    box-sizing: content-box;
}
#Index {
    width: 1200px;
    margin: 0 auto;
    padding: 60px 0;
    // 我要购买出售
    .tab {
        height: 70px;
        border-bottom: 1px solid #c9c9c9;
        margin-bottom: 40px;
        .tab-but {
            padding-left: 30px;
            width: 50%;
            float: left;
            box-sizing: border-box;
            > span {
                width: 158px;
                height: 44px;
                border-radius: 4px;
                line-height: 44px;
                background: #fff;
                display: inline-block;
                cursor: pointer;
                border: 1px solid #e8e8e8;
                margin-right: 22px;
                text-align: center;
            }
            .tab-active {
                background: #4895ea !important;
                border: 1px solid #4895ea !important;
                color: #fff;
            }
            span[data-v-c425e676]:last-child {
                margin-right: 0;
            }
        }
        .tab-right {
            float: left;
            width: 50%;
            padding-right: 30px;
            box-sizing: border-box;
            text-align: right;
            > a {
                width: 100px;
                height: 44px;
                border-radius: 4px;
                line-height: 44px;
                display: inline-block;
                text-align: center;
                color: #fff;
                margin-right: 20px;
                font-size: 17px;
                background: #4895ea;
                vertical-align: bottom;
            }
            > span {
                width: 60px;
                height: 44px;
                border-radius: 4px;
                line-height: 44px;
                background: #4895ea;
                display: inline-block;
                text-align: center;
                color: #fff;
                cursor: pointer;
                position: relative;
                > i {
                    font-size: 22px;
                }
                > em {
                    position: absolute;
                    top: 5px;
                    right: 10px;
                    background: #ff263e;
                    display: inline-block;
                    width: 16px;
                    height: 16px;
                    border-radius: 50%;
                    line-height: 16px;
                    font-size: 12px;
                }
            }
        }
    }
    .index {
        width: 1200px;
        overflow: hidden;
        padding-top: 18px;
        .index-conts {
            width: 2400px;
            transition-duration: 0.2s;
            .index-tab-list {
                width: 50%;
                float: left;
                //行情
                .price {
                    // border-top: 1px solid #c9c9c9;
                    // height: 50px;
                    .price-cont {
                        // margin-top: -15px;
                        > span {
                            width: 92px;
                            height: 28px;
                            border: 1px solid #8faed0;
                            display: inline-block;
                            text-align: center;
                            color: #fff;
                            background: #4895ea;
                            border-radius: 5px;
                            line-height: 28px;
                            font-size: 16px;
                            float: left;
                            margin: 0 60px 0 30px;
                        }
                        > ul {
                            /* float: left; */
                            display: flex;
                            flex-wrap: wrap;
                            // justify-content: space-around;
                            > li {
                                // float: left;
                                height: 28px;
                                border: 1px solid #d9d9d9;
                                line-height: 28px;
                                background: #fff;
                                border-radius: 5px;
                                padding: 0 10px;
                                // margin-right: 68px;
                                margin-right: 35px;
                                margin-bottom:15px; 
                                font-size: 13px;
                                cursor: pointer;
                                .index-green {
                                    color: #00cc5b;
                                }
                                .index-red {
                                    color: #ff0125;
                                }
                            }
                        }
                    }
                }
                // 交易列表
                .index-table {
                    background: #fff;
                    min-height: 480px;
                    > table {
                        width: 100%;
                        border-top: 1px solid #eee;
                        border-left: 1px solid #eee;
                        border-right: 1px solid #eee;
                        th {
                            font-size: 16px;
                            height: 66px;
                            line-height: 66px;
                            border-bottom: 1px solid #eee;
                            position: relative;
                            > i {
                                color: #69a4ed;
                            }
                            .tab-list {
                                box-shadow: 0 3px 9px 0 rgba(69, 112, 128, 0.14);
                                width: 110px;
                                position: absolute;
                                top: 55px;
                                left: 50%;
                                background: #fff;
                                border-radius: 4px;
                                margin-left: -55px;
                                overflow: hidden;
                                display: none;
                                z-index: 1;
                                li {
                                    font-size: 13px;
                                    cursor: pointer;
                                    line-height: 35px;
                                }
                            }
                        }
                        .tab-th:hover .tab-list {
                            display: block;
                        }
                        td {
                            height: 76px;
                            line-height: 76px;
                            text-align: center;
                            vertical-align: bottom;
                            border-bottom: 1px solid #eee;
                            position: relative;
                            .td-portrait {
                                width: 40px;
                                height: 40px;
                                color: #fff;
                                border-radius: 50%;
                                background: #4895ea;
                                display: inline-block;
                                line-height: 40px;
                                text-align: center;
                                margin-right: 5px;
                                font-size: 18px;
                                position: relative;
                                > i {
                                    position: absolute;
                                    bottom: 2px;
                                    right: 3px;
                                    display: inline-block;
                                    width: 8px;
                                    height: 8px;
                                    border-radius: 50%;
                                }
                                .on-line {
                                    background: #5dda07;
                                }
                            }
                            > span {
                                font-size: 14px;
                                padding: 0 5px;
                            }
                            .td-name {
                                font-size: 16px;
                                font-weight: bolder;
                                position: relative;
                                width: 140px;
                                display: inline-block;
                                text-align: left !important;
                            }
                            b {
                                position: absolute;
                                top: 15px;
                                left: 5px;
                                z-index: 1;
                                font-size: 12px;
                                color: #999;
                            }
                            > img {
                                width: 20px;
                                height: 20px;
                                margin: 0 5px;
                                vertical-align: -4px;
                            }
                            .USDT {
                                background: url(../../../static/imgs/USDT.png) 0
                                    28px no-repeat;
                                background-size: 20px 20px;
                            }
                            .BTC {
                                background: url(../../../static/imgs/BTC.png) 0
                                    28px no-repeat;
                                background-size: 20px 20px;
                            }
                            .ETH {
                                background: url(../../../static/imgs/ETH.png) 0
                                    28px no-repeat;
                                background-size: 20px 20px;
                            }
                            .td-currency {
                                padding-left: 25px !important;
                                display: inline-block;
                            }
                            > input {
                                width: 80px;
                                height: 30px;
                                border-radius: 4px;
                                outline: none;
                                cursor: pointer;
                                background: #4895ea;
                                color: #fff;
                                line-height: 1;
                            }
                        }
                    }
                }
            }
        }
    }
}
// 翻页
#page {
    text-align: center;
    .pagination {
        display: inline-block;
        padding-top: 30px;
        > li {
            float: left;
            width: 32px;
            height: 32px;
            border: 1px solid #eee;
            background: #fff;
            text-align: center;
            font-size: 14px;
            line-height: 32px;
            margin-left: 10px;
            border-radius: 4px;
            a {
                color: #555;
                display: inline-block;
                width: 100%;
                height: 100%;
            }
        }
        .disabled a {
            color: #eee !important;
        }
        .active a {
            color: #15b3f0 !important;
        }
    }
}
// 下单弹出
#IndexPop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.6);
    z-index: 10;
    // display: none;
    .index-pop {
        width: 474px;
        margin: 0 auto;
        padding-top: 100px;
        .pop-cont {
            background: #fff;
            box-shadow: 0 3px 9px 0 rgba(69, 112, 128, 0.14);
            padding: 20px;
            border-radius: 6px;
            .pop-head {
                font-size: 17px;
                position: relative;
                padding-bottom: 12px;
                > i {
                    position: absolute;
                    top: 0;
                    right: 0;
                    z-index: 1;
                    cursor: pointer;
                }
                > em {
                    font-size: 12px;
                    padding-left: 5px;
                    color: #24cf69;
                }
            }
            .pop-content {
                li {
                    font-size: 14px;
                    height: 50px;
                    line-height: 50px;

                    span {
                        padding: 0 30px;
                    }
                    img {
                        vertical-align: -6px;
                        padding: 0 5px;
                        height: 26px;
                        width: 26px;
                    }
                    .pop-back {
                        background: #ebebeb;
                        height: 38px;
                        line-height: 38px;
                        border-radius: 6px;
                    }
                    input {
                        background: #fff;
                        height: 24px;
                        width: 220px;
                        outline: none;
                    }
                }
                li:first-child > span {
                    padding-left: 0;
                }
                li:nth-child(2) > span {
                    padding-left: 0;
                }
                .pop-but {
                    text-align: center;
                    padding: 25px 0 10px;
                    > button {
                        width: 172px;
                        height: 46px;
                        outline: none;
                        margin: 0 15px;
                        border: 1px solid silver;
                        cursor: pointer;
                        border-radius: 6px;
                        background: #fff;
                        font-size: 16px;
                    }
                    .confirm {
                        background: #4895ea !important;
                        color: #fff;
                        border: 1px solid #4895ea !important;
                    }
                }
            }
        }
    }
}
// 消息
.news-prpmpt {
    position: fixed;
    top: 0;
    right: 0;
    z-index: 6;
    width: 400px;
    height: 100%;
    background: #fff;
    transition-duration: 0.2s;
    box-shadow: 0 3px 9px 0 rgba(69, 112, 128, 0.14);
    transform: translateX(110%);
    .news-header {
        height: 40px;
        background: #f8f8f8;
        line-height: 40px;
        text-align: center;
        font-size: 14px;
        position: relative;
        > span {
            font-weight: bolder;
        }
        > i {
            position: absolute;
            top: 0;
            right: 20px;
            font-size: 18px;
            cursor: pointer;
        }
    }
    .news-content {
        height: calc(100% - 80px);
        overflow: auto;
        > ul {
            padding: 10px 20px;
            > li {
                font-size: 12px;
                position: relative;
                padding: 5px 0;
                margin-bottom: 10px;
                i {
                    width: 8px;
                    height: 8px;
                    border-radius: 50%;
                    background: #999;
                    float: left;
                    position: absolute;
                    top: 10px;
                    left: 0;
                }
                .news-act {
                    background: #ff3838 !important;
                }
                span {
                    width: 96%;
                    display: inline-block;
                    float: left;
                    padding-left: 4%;
                    color: #323232;
                    line-height: 20px;
                    cursor: pointer;
                }
                time {
                    width: 100%;
                    text-align: right;
                    display: inline-block;
                    float: left;
                    color: #999;
                    margin-top: 5px;
                }
            }
            > li:after {
                content: "";
                clear: both;
                display: block;
            }
        }
        .nothings {
            color: #9b9b9b;
            font-size: 12px;
            padding-top: 80px;
            text-align: center;
            display: none;
        }
    }
    .news-footer {
        font-size: 14px;
        text-indent: 20px;
        line-height: 40px;
        cursor: pointer;
        color: #777;
        background: #f8f8f8;
        height: 40px;
    }
}
.msgpopup {
    transform: translateX(0%);
}
</style>
