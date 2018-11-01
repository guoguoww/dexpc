<template>
    <div class="app">
        <!-- 头 -->
        <tradeheader style="margin-bottom:10px;" />
        <div class="app-in">
            <!-- 左边 -->
            <div id="Tab" class="clearfix" v-show="!hl">
                <div class="money">
                    <div class="money-cont">
                        <p>{{coin}}余额：{{money}}</p>
                        <p>总保证金：{{totalprofit.amount}}</p>
                        <p>浮动盈亏：
                            <i :class="totalprofit.profit>0?'hong':'lv'">{{totalprofit.profit}}</i>
                        </p>
                        <em class="border-lefts"></em>
                        <em class="border-rights"></em>
                        <em class="border-bottoms"></em>
                        <em class="border-tops"></em>
                    </div>
                </div>
                <div class="tab-nav">
                    <div class="nav-cont">
                        <div class="nav-head">
                            <ul class="clearfix">
                                <li :class="{'nav-active':navacts==index}" v-for='(i,index) in typelist' :key="index" @click="navacts=index">
                                    <span>{{i}}</span>
                                </li>
                            </ul>
                        </div>
                        <div class="nav-list">
                            <ul>
                                <li :class="{'list-active':listactive==i}" v-for="(i,index) in list" :key='index' @click="listactive=i">
                                    <div class="nav-list-li">
                                        <img src="../../../static/imgs/BTC-icon.png">
                                        <h6>{{i.name}}</h6>
                                        <p>{{i.marginRate}} {{i.unitsCode}}/张</p>
                                        <p>最小波动:{{i.minFluctuate}} {{i.unitsCode}}</p>
                                    </div>
                                </li>
                            </ul>
                        </div>
                        <em class="border-lefts"></em>
                        <em class="border-rights"></em>
                        <em class="border-bottoms"></em>
                        <em class="border-tops"></em>
                    </div>
                </div>
            </div>
            <!-- 中间 -->
            <div id="chart" :style="{'margin-left': chartleft, 'margin-right': chartright}">
                <div class="chart-num"><img src="../../../static/imgs/left_hide.png" @click="hideleft"> <img @click="hideright" src="../../../static/imgs/right_hide.png">
                    <span>{{typelist[navacts]||'---/---'}}</span>
                    <span :class="market.diffRate>0?'hong':'lv'">{{market.price||'----'}}
                        <!-- <i class="iconfont icon-jiantou-copy-copy"></i> -->
                    </span>
                    <span>涨幅:
                        <em :class="market.diffRate>0?'hong':'lv'">{{market.diffRate||'----'}}%</em>
                    </span>
                    <span>开盘价:
                        <em>{{market.open||'----'}}</em>
                    </span>
                    <span>高:
                        <em>{{market.high||'----'}}</em>
                    </span>
                    <span>低:
                        <em>{{market.low||'----'}}</em>
                    </span>
                </div>

                <div class="chart">
                    <div class="chart-cont">
                        <div class="chart-cont-can">
                            <div id="chart_container" class="f-fill">
                                <!-- tradingview -->
                                <trading-view height='586px' :code='listactive.marketCode' @getPrice='getPrice' :market='market'></trading-view>
                            </div>

                        </div>
                        <em class="border-lefts"></em>
                        <em class="border-rights"></em>
                        <em class="border-bottoms"></em>
                        <em class="border-tops"></em>
                    </div>
                </div>
            </div>
            <!-- 右边 -->
            <div id="news" v-show="!hr">
                <div class="news-top">
                    <span>最新价</span>
                </div>
                <div class="news-list">
                    <div class="news-list-cont">
                        <ul class="clearfix">
                            <li>价格({{coin}})</li>
                            <li>时间</li>
                        </ul>
                        <div class="news-dl" v-for="(i,index) in  newPrice" :key="index" v-if="index<22">
                            <span class="hong">{{i.price}}</span>
                            <em>{{i.time}}</em>
                        </div>
                        <em class="border-lefts"></em>
                        <em class="border-rights"></em>
                        <em class="border-bottoms"></em>
                        <em class="border-tops"></em>
                    </div>
                </div>
            </div>
        </div>
        <div class="app-bottom clearfix">
            <div id="business" class="clearfix">
                <div class="business">
                    <div class="business-cont">
                        <div class="bus-top">
                            <dl class="clearfix">
                                <dt>
                                    <p>所需保证金：{{earnestmoney}}{{coin}}</p>
                                    <p>平仓手续费：{{orderlist.hands*listactive.fee}} {{coin}}</p>
                                </dt>
                                <dd>
                                    <div class="bus-num">
                                        <!-- <span>-</span> <input type="text">
                                        <span>+</span> -->
                                        <span @click="orderlist.hands--">-</span>
                                        <input min="0" v-model="orderlist.hands">
                                        <span @click="orderlist.hands++">+</span>

                                    </div>
                                </dd>
                            </dl>
                        </div>
                        <div class="bus-down">
                            <div class="tab-list">
                                <span>止盈</span>
                                <ul class="clearfix">
                                    <li :class="{'damage':orderlist.profitPoints===i.content}" @click="orderlist.profitPoints=i.content" v-for="(i,index) in StopProfit" :key="index">{{i.title}}</li>
                                </ul>
                            </div>
                            <div class="tab-list">
                                <span>止损</span>
                                <ul class="clearfix">
                                    <li :class="{'surplus':orderlist.lossPoints===i.content}" @click="orderlist.lossPoints=i.content" v-for="(i,index) in TrailingStop" :key="index">{{i.title}}</li>
                                </ul>
                            </div>
                        </div>
                        <div class="bus-but">
                            <input type="button" value="市价买多" class="damage" @click="confirm(0)">
                            <input type="button" value="市价卖空" class="surplus" @click="confirm(1)">
                        </div>
                        <em class="border-lefts"></em>
                        <em class="border-rights"></em>
                        <em class="border-bottoms"></em>
                        <em class="border-tops"></em>
                    </div>
                </div>

            </div>
            <div id="warehouse">
                <div class="warehouse">
                    <div class="warehouse-cont">
                        <div class="war-tab">
                            <span @click="waractive=index" :class="{'war-active':waractive==index}" v-for="(i,index) in warlist" :key="index">{{i}}</span>
                            <div class="war-whole clearfix">
                                <span>总保证金：{{totalprofit.amount}}</span>
                                <span>{{waractive==0?'浮动':'总'}}盈亏：
                                    <i :class="totalprofit.profit>0?'hong':'lv'">{{totalprofit.profit}}</i>
                                </span>
                                <span>总手续费：{{totalprofit.fees}}{{coin}}</span>
                                <em v-if="waractive==0">
                                    <i @click="sellall">全部平仓</i>
                                </em>
                            </div>
                        </div>
                        <div class="war-table" >
                            <table border="0" cellspacing="0" cellpadding="0">
                                <!-- <tr>
                                    <th>合约</th>
                                    <th>数量</th>
                                    <th>订单编号</th>
                                    <th>建仓时间</th>
                                    <th>保证金({{coin}})</th>
                                    <th>开仓价</th>
                                    <th>止盈价</th>
                                    <th>止损价</th>
                                    <th>盈亏({{coin}})</th>
                                    <th>操作</th>
                                </tr> -->

                            </table>
                            <div class="war-td" v-if="waractive==0&&userinfo">
                                <table border="0" cellspacing="0" cellpadding="0">
                                    <tr>
                                        <th>合约</th>
                                        <th>手数</th>
                                        <!-- <th>订单编号</th> -->
                                        <th>建仓时间</th>
                                        <th>下单方向</th>
                                        <th>保证金({{coin}})</th>
                                        <th>买入价</th>
                                        <th>止盈价</th>
                                        <th>止损价</th>
                                        <th>盈亏({{coin}})</th>
                                        <th>手续费</th>
                                        <th>操作</th>
                                    </tr>

                                    <tr v-for="(i,index) in tradelist" :key="index">
                                        <td>{{i.symbol}}</td>
                                        <td>{{i.hands }}</td>
                                        <!-- <td>{{i.serialNo}}</td> -->
                                        <td>{{i.buyTime}}</td>
                                        <td>{{i.direction==0?'买多':'卖空'}}</td>
                                        <td>{{i.orderPrice}}</td>
                                        <td>{{i.buyPoint }}</td>
                                        <td>{{i.profitPoints||'--'}}</td>
                                        <td>{{i.lossPoints||'--' }}</td>
                                        <td>{{i.fdyk}}</td>
                                        <td>{{i.fee}}{{coin}}</td>
                                        <td><button v-if="!i.profitPoints" class="sellbtn" @click="palmodel=true;tradeitem=i">设置止盈止损</button> <button class="sellbtn" @click="sell(i.id)">平仓</button></td>
                                    </tr>
                                </table>
                                <Page :total="trade.total" size="small" :page-size='trade.rows' @on-change='pagechange' v-if="trade.total>trade.rows" />
                            </div>
                            <div class="war-td" v-if="waractive==1&&userinfo">
                                <table border="0" cellspacing="0" cellpadding="0">
                                    <tr>
                                        <th>合约</th>
                                        <th>手数</th>
                                        <!-- <th>订单编号</th> -->
                                        <th>建仓时间</th>
                                        <th>下单方向</th>
                                        <th>保证金({{coin}})</th>
                                        <th>卖出时间</th>
                                        <th>买入价</th>
                                        <th>卖出价</th>
                                        <th>盈亏({{coin}})</th>
                                        <th>手续费</th>
                                        <!-- <th>操作</th> -->
                                    </tr>
                                    <tr v-for="(i,index) in tradelist" :key="index">
                                        <td>{{i.symbol}}</td>
                                        <td>{{i.hands }}</td>
                                        <!-- <td>{{i.serialNo}}</td> -->
                                        <td>{{i.buyTime}}</td>
                                        <td>{{i.direction==0?'买多':'卖空'}}</td>
                                        <td>{{i.orderPrice}}</td>
                                        <td>{{i.sellTime}}</td>
                                        <td>{{i.buyPoint }}</td>
                                        <td>{{i.sellPoint }}</td>
                                        <td>{{i.difMoney}}</td>
                                        <td>{{i.fee}}{{coin}}</td>
                                        <!-- <td>平仓</td> -->
                                    </tr>
                                </table>
                                <Page :total="trade.total" size="small" :page-size='trade.rows' @on-change='pagechange' v-if="trade.total>trade.rows" />
                            </div>
                            <div class="war-td" v-if="waractive==2&&userinfo">
                                <table border="0" cellspacing="0" cellpadding="0">
                                    <tr>
                                        <th>合约</th>
                                        <th>手数</th>
                                        <!-- <th>订单编号</th> -->
                                        <th>建仓时间</th>
                                        <th>下单方向</th>
                                        <th>保证金({{coin}})</th>
                                        <th>卖出时间</th>
                                        <th>买入价</th>
                                        <th>卖出价</th>
                                        <th>盈亏({{coin}})</th>
                                        <th>手续费</th>
                                        <!-- <th>操作</th> -->
                                    </tr>
                                    <tr v-for="(i,index) in tradelist" :key="index">
                                        <td>{{i.symbol}}</td>
                                        <td>{{i.hands }}</td>
                                        <!-- <td>{{i.serialNo}}</td> -->
                                        <td>{{i.buyTime}}</td>
                                        <td>{{i.direction==0?'买多':'卖空'}}</td>
                                        <td>{{i.orderPrice}}</td>
                                        <td>{{i.sellTime}}</td>
                                        <td>{{i.buyPoint }}</td>
                                        <td>{{i.sellPoint }}</td>
                                        <td>{{i.difMoney}}</td>
                                        <td>{{i.fee}}{{coin}}</td>
                                        <!-- <td>平仓</td> -->
                                    </tr>
                                    
                                </table>
                                <Page :total="trade.total" size="small" :page-size='trade.rows' @on-change='pagechange' v-if="trade.total>trade.rows" />
                            </div>
                            <div class="war-sign" v-if="!userinfo">
                                <span>您还没有登录，请
                                    <router-link to="/login">登录</router-link> 或
                                    <router-link to="/register">注册</router-link> 后再尝试</span>
                            </div>
                        </div>
                        <em class="border-lefts"></em>
                        <em class="border-rights"></em>
                        <em class="border-bottoms"></em>
                        <em class="border-tops"></em>
                    </div>
                </div>
                <!-- 历史 -->
                <!-- <div id="historyPop" v-if="waractive==2">
                    <div class="history-pop">
                        <div class="history-cont">
                            <i class="his-Cel" @click="waractive=0">×</i>
                            <div class="history-top">
                                <dl class="clearfix">
                                    <dt>
                                        <p>
                                            <em>Name:</em>
                                            <span>山东吴彦祖</span>
                                        </p>
                                        <p>
                                            <em>UID：</em>
                                            <span>DB020126376</span>
                                        </p>
                                    </dt>
                                    <dd><img src="../../../static/imgs/logo.png"></dd>
                                </dl>
                            </div>
                            <div class="history-tab">
                                <ul class="clearfix">
                                    <li class="his-act">历史交易</li>
                                    <li class="">历史资金</li>
                                </ul>
                            </div>
                            <div class="history-list list-one">
                                <table border="0" cellspacing="0" cellpadding="0">
                                    <tr>
                                        <th>订单编号</th>
                                        <th>品种名称</th>
                                        <th>买卖方向</th>
                                        <th>订单状态</th>
                                        <th>建仓价格</th>
                                        <th>平仓价格</th>
                                        <th>止盈价格</th>
                                        <th>止损价格</th>
                                        <th>委托数量</th>
                                        <th>保证金</th>
                                        <th>手续费</th>
                                        <th>隔夜费</th>
                                        <th>平仓盈亏</th>
                                        <th>净盈亏</th>
                                        <th>建仓时间</th>
                                        <th>平仓时间</th>
                                    </tr>
                                </table>
                            </div>

                            <em class="border-lefts"></em>
                            <em class="border-rights"></em>
                            <em class="border-bottoms"></em>
                            <em class="border-tops"></em>
                            <div id="page">
                                <ul class="pagination">
                                    <li class="disabled">
                                        <a href="javascript:;" class="iconfont icon-arrow-copy"></a>
                                    </li>
                                    <li class="active">
                                        <a href="javascript:;"> 1 </a>
                                    </li>
                                    <li class="disabled">
                                        <a href="javascript:;" class="iconfont icon-arrow"></a>
                                    </li>
                                </ul>
                            </div>
                            <div id="page" style="display: none;">
                                <ul class="pagination">
                                    <li class="disabled">
                                        <a href="javascript:;" class="iconfont icon-arrow-copy"></a>
                                    </li>
                                    <li class="active">
                                        <a href="javascript:;"> 1 </a>
                                    </li>
                                    <li class="disabled">
                                        <a href="javascript:;" class="iconfont icon-arrow"></a>
                                    </li>
                                </ul>
                            </div>
                        </div>
                    </div>
                </div> -->
            </div>
        </div>
        <Modal
            v-model="confirmshow"
            title="交易风险提示"
            @on-ok="order"
            class-name="vertical-center-modal"
            >
            <p>1、由于合约交易的点差机制，任何交易品种都会有三种价格，分别为“市价"“买多价”“卖空价”，买空价与卖空价分别是距市价加或减市价的万分之十二的距离，当用户对交易品种进行买多时以”买多价“为成本价进行成交，此时”卖空价“即为您的平仓结算价，包含您设置止损止盈时也请以”卖空价“为准；当用户对交易品种进行卖空时以”卖空价“为成本价成交，此时”买多价“即为您的平仓结算价，包含您设置止损止盈时也请以”买多价“为准</p>
            <p>2、当您的账户风险率达到或低于70%时，为避免您的账户因行情原因造成更大的损失，系统将会强制平仓您的所有持有订单，账户风险率计算方式为：账户净值/占用USDT*100%</p>
            <p>3、数字资产合约交易具有杠杆存在，交易损失风险可能性很大，有可能导致您损失账户内部分或全部USDT，因此您应该根据您的财务状况仔细考虑数字资产合约杠杆交易是否适合您</p>
            <br>
            <p>是否确认<b style="font-weight: bolder;">{{orderlist.direction==0?'买多':'卖空'}}{{listactive.name}}*{{orderlist.hands}}</b>?</p>
        </Modal>
        <Modal
            v-model="palmodel"
            title="设置止盈止损"
            @on-ok="setpal"
            class-name="vertical-center-modal">
                <div class="bus-down">
                    <div class="tab-list">
                        <span>止盈</span>
                        <ul class="clearfix" style="border: 2px solid #0079df;">
                            <li style="border-right: 2px solid #0079df;color: #0079df;background: #fff;" :class="{'damage1':pal.profitPoints===i.content}" @click="pal.profitPoints=i.content" v-for="(i,index) in profitPoints" :key="index">{{i.title}}</li>
                        </ul>
                    </div>
                    <div class="tab-list">
                        <span>止损</span>
                        <ul class="clearfix" style="border: 2px solid #0079df;">
                            <li style="border-right: 2px solid #0079df;color: #0079df;background: #fff;" :class="{'surplus1':pal.lossPoints===i.content}" @click="pal.lossPoints=i.content" v-for="(i,index) in lossPoints" :key="index">{{i.title}}</li>
                        </ul>
                    </div>
                </div>
        </Modal>
    </div>
</template>

<script type="">
import tradeheader from "../common/tradeheader";
import tradingView from "../tradingView/index";

export default {
    name: "contract",
    data() {
        return {
            userinfo:this.$cookie.get('_auth'),
            typelist: [], //合约类型列表
            navacts: 0, //navbar 选择
            list: [], //合约列表
            listactive: {}, //当前选择的合约
            markettimer: null, //行情定时器
            marketall:[],//全部行情
            market: {}, //行情
            money: 0, //usdt余额
            totalprofit: {
                //总盈亏
                amount: 0,//保证金
                profit: 0,//手续费
                fees:0,//手续费
            },
            StopProfit: [], //止盈
            TrailingStop: [], //止损列表
            orderlist: {
                //下单列表
                contractId: "", //合约ID
                direction: "", //方向 0:买涨 1:买跌
                hands: 0, //手数
                lossPoints: "", //止损点(如 50% 为0.5)
                profitPoints: "" //止盈点(如 50% 为0.5)
            },
            chartleft: "244px",
            chartright: "180px",
            hl: false, //隐藏左边
            hr: false, //隐藏右边
            tradelist: [], //订单列表
            trade:{//订单页数总数
                rows:10,
                start:0,
                total:0
            },
            waractive: 0, //当日持仓选择
            warlist: ["当日持仓", "当日订单", "历史明细"], //当日持仓选项
            confirmshow:false,///下单确认框
            newPrice:[],//最新价
            palmodel:false,//设置止盈止损model
            tradeitem:{},//当前设置止盈止损的那条数据
            pal:{//止盈止损参数
                tradeId:'',
                lossPoints:0.2,
                profitPoints:0.1
            },
            profitPoints:[//止盈列表
                {title:'10%',content:0.1},
                {title:'20%',content:0.2},
                {title:'40%',content:0.4},
                {title:'60%',content:0.6},
                {title:'80%',content:0.8},
                {title:'100%',content:1.0},
            ],
            lossPoints:[//止损列表
                {title:'20%',content:0.2},
                {title:'30%',content:0.3},
                {title:'40%',content:0.4},
                {title:'50%',content:0.5},
                {title:'60%',content:0.6},
                {title:'70%',content:0.7},
            ]
        };
    },
    components: {
        tradeheader,tradingView
    },
    created() {
        this.gettype();
        this.getmoney();
        // this.gettotalprofit();
        this.gettradelist();
    },
    beforeDestroy() {
        clearInterval(this.markettimer);
        // clearInterval(this.fstimer);
        this.markettimer = null;
        // this.fstimer=null;
    },
    computed: {
        earnestmoney() {
            //保证金
            return this.listactive.marginRate * this.orderlist.hands;
        },
        //当前合约货币
        coin(){
            if (this.typelist.length) {
                return this.typelist[this.navacts].split('/')[1]
            }
           return '--'
        },
    },
    watch: {
        coin(){
            this.getmoney()
        },
        navacts() {
            //nav切换时重新请求列表
            this.getlist();
            this.setmarkettimer();
            // this.selectMarket()
        },
        // 手数
        "orderlist.hands"(v) {
            if (v < 0) {
                this.orderlist.hands = 0;
            }
            if (+v > +this.listactive.sumHandsLimit) {
                this.orderlist.hands = +this.listactive.sumHandsLimit;
            }
        },
        waractive(){//当日持仓当日订单
            this.gettradelist();
        }
    },
    methods: {
        //隐藏侧边栏
        hideleft() {
            if (this.hl == true) {
                this.chartleft = "244px";
                this.hl = false;
                return;
            }
            this.chartleft = "10px";
            this.hl = true;
        },
        hideright() {
            if (this.hr == true) {
                this.chartright = "180px";
                this.hr = false;
                return;
            }
            this.chartright = "10px";
            this.hr = true;
        },
        //获取合约类型
        gettype() {
            this.$axios
                .post("api/trade/contract/type")
                .then(res => {
                    if (!res.data.errorCode) {
                        this.typelist = res.data.data;
                        this.getlist();
                    }
                })
                .catch(err => {});
        },
        //获取合约列表
        getlist() {
            this.$axios
                .post(
                    `api/trade/contract/list_query?contractType=${
                        this.typelist[this.navacts]
                    }`
                )
                .then(res => {
                    if (!res.data.errorCode) {
                        this.list = res.data.data;
                        this.listactive = res.data.data[0];
                        console.log(this.listactive);
                        // this.setfstimer();
                        this.setmarkettimer();
                        this.getStopProfit();
                        this.getTrailingStop();
                    }
                })
                .catch(err => {});
        },
        //行情定时器
        setmarkettimer() {
            if (this.markettimer) {
                clearInterval(this.markettimer);
                this.markettimer = null;
                this.getmarket();
            } else {
                this.getmarket();
            }
            this.markettimer = setInterval(() => {
                this.getmarket();
                // this.gettotalprofit();
            }, 1000);
        },
        //获取行情
        getmarket() {
            var obj={}
            var arr=[]
            // 获取行情code
            this.tradelist.forEach((item,index)=>{
                //console.log(item.marketCode);
                if (!obj[item.marketCode]) {
                    obj[item.marketCode]=item.marketCode
                    arr.push(obj[item.marketCode])
                }
            })

            if (!obj.hasOwnProperty(this.listactive.marketCode)) {
                     arr.push(this.listactive.marketCode)
            }
            console.log(arr.join(','),'latest_market');
            if (!arr.join(',')) {
                return
            }
            this.$axios.get(`api/market/latest_market?codes=${arr.join(',')}`).then(res => {
                    if (!res.data.errorCode) {
                        this.marketall = res.data.data;
                        this.selectMarket(); //选择行情
                        this.setprice()
                        // console.log(this.market);
                    }
                }).catch(err => {

                });
        },
        //获取当前行情
        selectMarket() {
            this.marketall.forEach((i, index) => {
                if (i.code == this.listactive.marketCode) {
                    this.market = i;
                }
            });
        },
        // 设置最新价格 浮动盈亏
        setprice(){
            // totalprofit: {
            //     //总盈亏
            //     amount: 0,//保证金
            //     profit: 0,//手续费
            //     fees:0,//手续费
            // },
            var totalprofit={
                amount: 0,//保证金
                profit: 0,//手续费
                fees:0,//手续费
            }
            if(this.waractive==0){
               var profit='fdyk'
            }else{
                var profit='difMoney'
            }
            this.tradelist.forEach((item,index)=>{
                this.marketall.forEach((i) => {
                    if (item.marketCode==i.code) {
                        this.$set(item,'price',i.price)
                    }
                    if(item.direction== '0'){//当前价 - 建仓价 （买涨）
                        this.$set(item , 'fdyk', Number(item.hands*((item.price) - (item.buyPoint) )* item.pointPrice).toFixed(2));
                    }else if (item.direction == '1'){//建仓价 - 当前价 （买跌）
                        this.$set(item , 'fdyk', Number(item.hands*((item.buyPoint) - (item.price) )* item.pointPrice).toFixed(2));
                    }
                })
                totalprofit.fees+=item.fee
                totalprofit.amount+=item.orderPrice
    
                totalprofit.profit+=(+item[profit])
                console.log(profit);
                
                
            })

            totalprofit.profit=totalprofit.profit.toFixed(4)
            Object.assign(this.totalprofit,totalprofit)
            // this.totalprofit.fees=fees
            // console.warn(this.totalprofit.fees);
        },
        //获取账户余额
        getmoney() {
            if (this.$cookie.get("_auth")) {
                this.$axios.post(`api/assets/query?type=2`).then(res => {
                    if (!res.data.errorCode) {
                        console.log(res);
                        res.data.data.forEach(item => {
                            console.log(item);
                            if (item.currencyCode == this.coin) {
                                this.money = item.amount;
                            }
                        });
                    }
                });
            }
        },
        //获取总盈亏
        gettotalprofit() {
            if (!this.$cookie.get("_auth")) {
                return;
            }
            // this.$axios
            //     .post("api/trade/contract/statistics")
            //     .then(res => {
            //         console.log(res);
            //         if (!res.data.errorCode) {
            //             this.totalprofit = res.data.data;
            //         }
            //     })
            //     .catch(err => {});
        },
        //获取止盈
        getStopProfit() {
            var arr = [];
            var maxprofit = 1 * 100;
            var profitPoints = (+this.listactive.profitPoints).toFixed(1);
            this.orderlist.profitPoints = profitPoints;
            for (let i = 20; i <= maxprofit; i += 20) {
                arr.push({
                    title: `${i}%`,
                    content: i / 100
                });
            }
            arr.unshift({
                title: "默认",
                content: profitPoints
            });
            console.log(arr);
            this.StopProfit = arr;
        },
        //获取止损列表
        getTrailingStop() {
            var arr = [];
            var maxloss = this.listactive.maxLossPoints * 100;
            var lossPoints = (+this.listactive.lossPoints).toFixed(1);
            this.orderlist.lossPoints = lossPoints;
            for (let i = 30; i <= maxloss; i += 10) {
                arr.push({
                    title: `${i}%`,
                    content: i / 100
                });
            }
            arr.unshift({
                title: "默认",
                content: lossPoints
            });
            console.log(arr);
            this.TrailingStop = arr;
            // return arr
        },
        //下单确认框
        confirm(direction){
            if (!this.orderlist.hands) {
                this.$Message.warning("委托数量错误");
                return;
            }
            this.orderlist.direction = direction;
            this.orderlist.contractId = this.listactive.id;
            this.confirmshow=true
        },
        //下单
        order() {
            
            console.log(this.orderlist);

            this.$axios({
                url: "api/trade/contract/place_order",
                method: "post",
                data: this.orderlist
            }).then(res => {
                if (!res.data.errorCode) {
                    this.$Message.success("下单成功!");
                    // this.gettotalprofit();
                    this.gettradelist();
                }
            });
        },
        //全部平仓
        sellall(){
            const arr=[]
            this.tradelist.forEach((item,index)=>{
                arr.push(item.id)
            })
            console.log(arr);
            this.sell(arr.join(','))
        },
        //平仓
        sell(id){
            this.$axios.post(`api/trade/contract/do_sell?tradeId=${id}`).then(res=>{
                if (!res.data.errorCode) {
                    this.$Message.success("平仓成功!");
                    // this.gettotalprofit();
                    this.gettradelist();
                }
            }).catch(err=>{

            })
        },
        //获取订单列表
        gettradelist() {
            if (!this.$cookie.get("_auth")) {
                return;
            }
            this.$axios
                .post("api/trade/contract/trade_query", {
                    status: this.waractive,
                    start: this.trade.start,
                    rows: this.trade.rows
                })
                .then(res => {
                    if (!res.data.errorCode) {
                        this.tradelist = res.data.page.data;
                        this.trade.total=res.data.page.total
                        // console.warn(this.tradelist);
                        
                        this.getmarket();
                        console.log(this.tradelist);
                    }
            });
        },
        //翻页
        pagechange(index){
            console.log(index);
            this.tradelist=[]
            this.trade.start=(index-1)*this.trade.rows
            this.gettradelist()
            
        },
        // 获取最新价格
        getPrice(data){
            const arr=[]
            data.map(val=>{
                arr.push({
                    price:+val.close,
                    time:val.time.split(' ')[1]
                })
            })
            this.newPrice=arr.reverse()
            console.log('getPrice',data);
            
            // console.log('getPrice',arr.reverse());
        },
        //设置止盈止损
        setpal(){
            this.palmodel=false
            this.pal.tradeId=this.tradeitem.id
            this.$axios.post('api/trade/contract/set_up_stop',this.pal).then(res=>{
                if (!res.data.errorCode){
                    this.$Message.success("设置成功!");
                    this.gettradelist();
                }
            })
        }
    }
};
</script>
 
<style scoped lang="less">
.damage1 {
    background: #c31e36 !important;
    color: #fff !important;
}
.surplus1 {
    background: #00802b !important;
    color: #fff !important;
}

* {
    box-sizing: content-box;
}
.vertical-center-modal{
        display: flex;
        align-items: center;
        justify-content: center;
        
        .ivu-modal{
            top: 0;
        }
}

.app {
    height: 100%;
    min-width: 1300px;
    background-color: #000;
    color: #fff;
}

.border-lefts,
.border-rights,
.border-bottoms,
.border-tops {
    display: inline-block;
    width: 8px;
    height: 8px;
    position: absolute;
}
.border-lefts {
    border-bottom: 2px solid #188abe;
    border-left: 2px solid #188abe;
    bottom: -1px;
    left: -1px;
}
.border-rights {
    border-top: 2px solid #188abe;
    border-right: 2px solid #188abe;
    right: -1px;
    top: -1px;
}
.border-bottoms {
    border-bottom: 2px solid #188abe;
    border-right: 2px solid #188abe;
    right: -1px;
    bottom: -1px;
}
.border-tops {
    border-top: 2px solid #188abe;
    border-left: 2px solid #188abe;
    top: -1px;
    left: -1px;
}
.war-sign{
    text-align: center;
    line-height: 180px;
    font-size: 12px;
    >span{
    padding-left: 36px;
    height: 26px;
    line-height: 26px;
    background: url(../../../static/imgs/smile.png) no-repeat 0 0;
    background-size: 26px 26px;
    display: inline-block;
    >a{
    color: #1daaec;
}
}
}
.sellbtn{
    border: 1px solid #0079df;
    border-radius: 4px;
    color: #0079df;
    font-size: 12px;
    /* width: 65px; */
    height: 18px;
    line-height: 18px;
    display: inline-block;
    text-align: center;
    cursor: pointer;
    background: #000;
    padding: 0px 5px;
    background: #000;
}
.app-in {
    position: relative;
    // 左边
    #Tab {
        padding-left: 14px;
        width: 236px;
        box-sizing: border-box;
        position: absolute;
        top: 0;
        left: 0;
        .money,
        .tab-nav {
            width: 100%;
            border: 1px solid #0a3b4c;
            margin-top: 4px;
            padding-bottom: 4px;
            padding-right: 4px;
            box-sizing: border-box;
            margin-bottom: 20px;
        }
        .money {
            .money-cont {
                border: 1px solid #0a3b4c;
                margin-top: -4px;
                margin-left: -4px;
                background: #000;
                padding: 14px 0 14px 25px;
                position: relative;
                > p {
                    line-height: 25px;
                    font-size: 14px;
                }
            }
        }
        .tab-nav {
            .nav-cont {
                border: 1px solid #0a3b4c;
                margin-top: -4px;
                margin-left: -4px;
                background: #000;
                position: relative;
                padding-bottom: 20px;
                .nav-head {
                    padding: 15px 16px;
                    li {
                        float: left;
                        width: 50%;
                        text-align: center;
                        font-size: 13px;
                        height: 22px;
                        line-height: 22px;
                        background: #01080b;
                        background: linear-gradient(
                            180deg,
                            #01080b 0,
                            #082d3e 28%,
                            #09374c 50%,
                            #082d3e 80%,
                            #01080b
                        );
                        filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#01080b",endColorstr="#01080b",GradientType=0);
                        > span {
                            display: inline-block;
                            background: #000;
                            width: calc(100% - 2px);
                            cursor: pointer;
                        }
                    }
                    .nav-active > span {
                        color: #f4f100;
                    }
                }
                .nav-list {
                    margin: 0 10px;
                    height: 436px;
                    overflow: hidden;
                    overflow-y: scroll;
                    > ul {
                        background: #000;
                        background: linear-gradient(
                            90deg,
                            #000 0,
                            #083248 20%,
                            #1baef3 40%,
                            #1eabf0 53%,
                            #1baef3 67%,
                            #083248 87%,
                            #000
                        );
                        filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#000000",endColorstr="#000000",GradientType=1);
                        padding: 1px 0;
                        li {
                            background: #000;
                            padding: 3px 0;
                            margin-bottom: 1px;
                            .nav-list-li {
                                text-align: right;
                                line-height: 26px;
                                padding: 13px 30px;
                                cursor: pointer;
                                position: relative;
                                > img {
                                    position: absolute;
                                    top: 15px;
                                    left: 28px;
                                    z-index: 1;
                                    width: 48px;
                                }
                                > h6 {
                                    font-size: 15px;
                                }
                                > p {
                                    font-size: 12px;
                                }
                            }
                        }
                        .list-active .nav-list-li {
                            background: #000;
                            background: linear-gradient(
                                90deg,
                                #000 0,
                                #030f15 20%,
                                #082c3d 39%,
                                #093246 53%,
                                #082c3d 69%,
                                #030f15 87%,
                                #000
                            );
                            filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#000000",endColorstr="#000000",GradientType=1);
                        }
                    }
                }
            }
        }
    }
    // 中间
    #chart {
        position: relative;
        .chart-num {
            padding: 0 36px 25px;
            height: 28px;
            line-height: 28px;
            font-size: 15px;
            position: relative;
            > img {
                width: 23px;
                position: absolute;
                top: 3px;
                cursor: pointer;
            }
            > img:first-child {
                left: 10px;
            }
            > img:nth-child(2) {
                right: 10px;
            }
            > span {
                display: inline-block;
                text-align: center;
                padding: 0 1.8%;
                > i {
                    font-size: 14px;
                    padding-left: 2px;
                }
                > em {
                    padding-left: 5px;
                }
            }
        }
        .chart {
            border: 1px solid #0a3b4c;
            padding-bottom: 4px;
            padding-right: 4px;
            box-sizing: border-box;
            margin-left: 4px;
            margin-bottom: 20px;
            .chart-cont {
                border: 1px solid #0a3b4c;
                margin-top: -4px;
                margin-left: -4px;
                position: relative;
                height: 586px;
                background: #000;
                .chart-cont-can {
                    width: 100%;
                    height: 100%;
                    background: url(../../../static/imgs/bg.png) no-repeat 50%
                        50%;
                    background-size: 130px 136px;
                    position: relative;
                }
                .f-fill {
                    width: 100%;
                    height: 100%;
                }
            }
        }
    }
    // 右边
    #news {
        width: 170px;
        padding-right: 10px;
        box-sizing: border-box;
        position: absolute;
        top: 0;
        right: 0;
        .news-top {
            font-size: 17px;
            color: #1987da;
            height: 24px;
            > span {
                background: url(../../../static/imgs/computer.png) no-repeat 0 0;
                background-size: 22px;
                display: inline-block;
                padding-left: 36px;
                height: 24px;
            }
        }
        .news-list {
            margin-top: 28px;
            width: 100%;
            border: 1px solid #0a3b4c;
            padding-bottom: 4px;
            padding-right: 4px;
            box-sizing: border-box;
            .news-list-cont {
                border: 1px solid #0a3b4c;
                margin-top: -4px;
                margin-left: -4px;
                background: #000;
                position: relative;
                padding: 10px 12px;
                height: 567px;
                > ul > li {
                    float: left;
                    width: 50%;
                    font-size: 12px;
                    line-height: 25px;
                    text-align: center;
                }
                .news-dl > em,
                .news-dl > span {
                    float: left;
                    width: 50%;
                    font-size: 12px;
                    line-height: 25px;
                    display: inline-block;
                }
                .news-dl > em {
                    text-align: right;
                }
            }
        }
    }
}
.app-bottom {
    // 右边
    #business {
        float: right;
        padding: 0 10px;
        width: 33%;
        box-sizing: border-box;
        .business {
            border: 1px solid #0a3b4c;
            padding-bottom: 4px;
            padding-right: 4px;
            box-sizing: border-box;
            margin-left: 4px;
            margin-bottom: 10px;
            float: left;
            width: calc(100% - 4px);
            .business-cont {
                border: 1px solid #0a3b4c;
                margin-top: -4px;
                margin-left: -4px;
                position: relative;
                background: #000;
                padding: 25px 10px 20px;
                height: 244px;
                box-sizing: border-box;
                .bus-top dt {
                    float: left;
                    width: 50%;
                    font-size: 13px;
                    > p {
                        line-height: 22px;
                        padding-left: 20px;
                    }
                }
                .bus-top dd {
                    float: left;
                    width: 50%;
                    padding: 5px 0;
                    .bus-num {
                        width: 90%;
                        border: 1px solid #0a3b4c;
                        border-radius: 20px;
                        height: 30px;
                        overflow: hidden;
                        position: relative;
                        > span {
                            display: inline-block;
                            width: 40px;
                            cursor: pointer;
                            text-align: center;
                            font-size: 25px;
                            position: absolute;
                            top: 0;
                            left: calc(100% - 40px);
                            height: 30px;
                            line-height: 28px;
                            color: #0a3b4c;
                            border-left: 1px solid #0a3b4c;
                        }
                        > span:first-child {
                            left: 0;
                            border-right: 1px solid #0a3b4c;
                            border-left: 0;
                        }
                        > input {
                            margin: 1px 40px;
                            width: calc(100% - 80px);
                            height: 28px;
                            text-align: center;
                            outline: none;
                            font-size: 14px;
                            color: #fff;
                            background: #000;
                        }
                    }
                }
                // .bus-down {
                //     .tab-list {
                //         padding: 15px 18px 0;
                //         position: relative;
                //         > span {
                //             display: inline-block;
                //             width: 45px;
                //             height: 30px;
                //             line-height: 30px;
                //             font-size: 15px;
                //             color: #005db0;
                //             position: absolute;
                //             top: 15px;
                //             left: 18px;
                //             z-index: 1;
                //         }
                //         > ul {
                //             margin-left: 45px;
                //             border: 2px solid #072a39;
                //             border-radius: 4px;
                //             overflow: hidden;
                //             > li {
                //                 float: left;
                //                 width: 16.666%;
                //                 height: 30px;
                //                 line-height: 30px;
                //                 font-size: 15px;
                //                 color: #fff;
                //                 text-align: center;
                //                 cursor: pointer;
                //                 background: #062432;
                //                 border-right: 2px solid #000203;
                //                 box-sizing: border-box;
                //             }
                //         }
                //     }
                // }
                .bus-but {
                    padding-top: 25px;
                    text-align: center;
                    > input {
                        height: 30px;
                        font-size: 16px;
                        color: #fff;
                        outline: none;
                        border: 0;
                        width: 30%;
                        border-radius: 30px;
                        cursor: pointer;
                        margin-right: 10%;
                    }
                    > input:last-child {
                        margin-right: 0;
                    }
                }
            }
        }
    }
    
    // 左边
    #warehouse {
        float: left;
        width: 67%;
        padding: 0 0 0 10px;
        box-sizing: border-box;
        .warehouse {
            border: 1px solid #0a3b4c;
            padding-bottom: 4px;
            padding-right: 4px;
            box-sizing: border-box;
            margin-left: 4px;
            margin-bottom: 10px;
            .warehouse-cont {
                .war-td{
                    >ul{
                        width: 100%;
                        text-align: center;
                    }
                }
                border: 1px solid #0a3b4c;
                margin-top: -4px;
                margin-left: -4px;
                position: relative;
                background: #000;
                padding: 10px;
                .war-tab {
                    height: 42px;
                    > span {
                        display: inline-block;
                        width: 74px;
                        height: 23px;
                        text-align: center;
                        font-size: 13px;
                        background: #062432;
                        line-height: 23px;
                        border-radius: 4px;
                        margin: 10px 5px 0;
                        cursor: pointer;
                    }
                    .war-active {
                        background: #2b4461 !important;
                    }
                    .war-whole {
                        position: absolute;
                        top: 20px;
                        right: 10px;
                        background: #031016;
                        background: linear-gradient(
                            180deg,
                            #031016 0,
                            #051c28 12%,
                            #083144 30%,
                            #0d4b68 46%,
                            #0d4b68 55%,
                            #083144 70%,
                            #051c28 89%,
                            #031016
                        );
                        filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#031016",endColorstr="#031016",GradientType=0);
                        > span {
                            float: left;
                            display: inline-block;
                            padding: 0 10px;
                            font-size: 12px;
                            color: #fff;
                            background: #000;
                            height: 20px;
                            line-height: 20px;
                            margin-left: 1px;
                        }
                        > em {
                            float: left;
                            padding: 0 30px;
                            margin-left: 1px;
                            background: #000;
                            > i {
                                border: 1px solid #0079df;
                                border-radius: 4px;
                                color: #0079df;
                                font-size: 12px;
                                width: 65px;
                                height: 18px;
                                line-height: 18px;
                                display: inline-block;
                                text-align: center;
                                cursor: pointer;
                                background: #000;
                            }
                        }
                    }
                }
                .war-table {
                    table {
                        width: 100%;
                        th {
                            height: 30px;
                            background: #062432;
                            font-size: 12px;
                            line-height: 30px;
                            text-align: center;
                        }
                        // th:first-child {
                        //     width: 10%;
                        // }
                        // th:nth-child(2) {
                        //     width: 4%;
                        // }
                        // th:nth-child(2) {
                        //     width: 4%;
                        // }
                        // th:nth-child(2) {
                        //     width: 4%;
                        // }
                        // th:nth-child(3) {
                        //     width: 12%;
                        // }
                        // th:nth-child(4) {
                        //     width: 17%;
                        // }
                        // th:nth-child(5) {
                        //     width: 10%;
                        // }
                        // th:nth-child(6),
                        // th:nth-child(7),
                        // th:nth-child(8) {
                        //     width: 9%;
                        // }
                        // th:nth-child(10) {
                        //     width: 10%;
                        // }
                    }
                    .war-td {
                        /* margin-top: 10px; */
                        height: 180px;
                        overflow: auto;
                        table {
                            width: 100%;
                            td {
                                text-align: center;
                                line-height: 30px;
                            }
                        }
                    }
                }
            }
        }
    }
}
                .bus-down {
                    .tab-list {
                        padding: 15px 18px 0;
                        position: relative;
                        > span {
                            display: inline-block;
                            width: 45px;
                            height: 30px;
                            line-height: 30px;
                            font-size: 15px;
                            color: #005db0;
                            position: absolute;
                            top: 15px;
                            left: 18px;
                            z-index: 1;
                        }
                        > ul {
                            margin-left: 45px;
                            border: 2px solid #072a39;
                            border-radius: 4px;
                            overflow: hidden;
                            > li {
                                float: left;
                                width: 16.666%;
                                height: 30px;
                                line-height: 30px;
                                font-size: 15px;
                                color: #fff;
                                text-align: center;
                                cursor: pointer;
                                background: #062432;
                                border-right: 2px solid #000203;
                                box-sizing: border-box;
                            }
                        }
                    }
                }
// 历史订单
#historyPop {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 10;
    background: hsla(0, 0%, 56%, 0.6);
    .history-pop {
        position: absolute;
        width: 1000px;
        top: 50%;
        left: 50%;
        margin-left: -500px;
        margin-top: -360px;
        border: 1px solid #0a3b4c;
        box-sizing: border-box;
        padding-bottom: 4px;
        padding-right: 4px;
        .history-cont {
            border: 1px solid #0a3b4c;
            position: relative;
            background: #000;
            margin-top: -4px;
            margin-left: -4px;
            .his-Cel {
                position: absolute;
                top: -25px;
                right: -25px;
                width: 25px;
                height: 25px;
                border-radius: 50%;
                background: #000;
                border: 1px solid #0a3b4c;
                line-height: 25px;
                text-align: center;
                font-size: 20px;
                cursor: pointer;
            }
            .history-top {
                padding: 15px 0;
                dd,
                dt {
                    float: left;
                    width: 50%;
                    box-sizing: border-box;
                    font-size: 12px;
                }
                dt {
                    padding-left: 45px;
                    > p > em {
                        display: inline-block; // width: 32px;
                        position: relative;
                        height: 16px; // text-indent: 27px;
                        > i {
                            position: absolute;
                            top: 0;
                            left: -23px;
                            font-size: 16px;
                        }
                    }
                    > p:last-child {
                        padding-top: 6px;
                        > em {
                            text-indent: 0;
                        }
                    }
                }
                dd {
                    padding-right: 45px;
                    text-align: right;
                    > img {
                        height: 30px;
                    }
                }
            }
            .history-tab {
                height: 34px;
                background: #062432;
                padding: 3px 30px;
                box-sizing: border-box;
                > ul {
                    display: inline-block;
                    border: 1px solid #0a0204;
                    border-radius: 4px;
                    overflow: hidden;
                    > li {
                        float: left;
                        width: 84px;
                        height: 25px;
                        line-height: 25px;
                        text-align: center;
                        font-size: 14px;
                        cursor: pointer;
                        .his-act {
                            background: #2b4461;
                        }
                    }
                }
            }
            .history-list {
                padding: 12px 16px;
                height: 598px;
                overflow: auto;
                position: relative;
                margin-bottom: 34px;
                > table th {
                    font-size: 13px;
                    color: #1986d9;
                    height: 32px;
                    line-height: 32px;
                }
            }
            .list-one table {
                width: 1300px !important;
            }
        }
    }
    #page {
        text-align: center;
        position: absolute;
        left: 0;
        right: 0;
        bottom: 5px;
        z-index: 1;
        .pagination {
            display: inline-block;
            > li {
                float: left;
                width: 20px;
                height: 20px;
                border: 1px solid #999;
                text-align: center;
                font-size: 12px;
                line-height: 20px;
                margin-left: 8px;
                border-radius: 4px;
                a {
                    color: #fff;
                    display: inline-block;
                    width: 100%;
                    height: 100%;
                }
            }
            .active {
                border: 1px solid #35578f !important;
            }
            .disabled a {
                color: #eee !important;
            }
        }
    }
}
</style>
