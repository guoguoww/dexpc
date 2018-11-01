<template>
    <div class="trade_main">
        <!-- 头 -->
        <tradeheader/>
        <!-- 交易主体 -->
        <div id="trade_body">
            <!-- 左边 -->
            <div id="left" v-show="!hideleft">
                <!-- <div class="sorts">
                    <ul class="sorts_top">
                        <li class="sorts-active">
                            <span>USDT</span>
                        </li>
                        <li class="">
                            <span>BTC</span>
                        </li>
                        <li>
                            <span> <img style="margin-top:-4px;" src="../../../static/imgs/self_profile.png"></span>
                        </li>
                    </ul>
                    <p class="sorts-line"></p>
                    <div class="search">
                        <p><input></p>
                        <p>CNY</p>
                    </div>
                </div> -->
                <div class="main_area">

                    <div class="data">
                        <ul class="sorts_bottom" style="padding-bottom:20px;">
                            <button>主场</button>
                            <button>最新价</button>
                            <button>涨跌幅</button>
                        </ul>
                        <p class="left-line"></p>
                        <ul class="maketlist">
                            <li class="maketlist-left " v-for="(i,index) in category" :key="index" :class="{'maketlist-left-bg':i==coinitem}" @click="coinitem=i">

                                <div class="list-left">
                                    <img alt=""  src="../../../static/imgs/white_profile.png">
                                    <div class="list-icon" >
                                        <img alt="" :src='`${path}static/imgs/${i.exchangeCode}-icon.png`'>
                                        <h6 >{{i.exchangeCode||'----' }}/{{i.unitsCode||'----' }}</h6>
                                    </div>
                                    <div class="list-left-right" >
                                        <h2 class="depreciate-class"  :class="i.diffRate>0?'hong':'lv'">{{i.diffRate||'----'}}%</h2>
                                        <p  class="depreciate-class" :class="i.diffRate>0?'hong':'lv'">{{i.diff||'----'}}</p>
                                    </div>
                                </div>
                                <p class="left-line"></p>

                            </li>


                        </ul>
                    </div>
                </div>

            </div>
            <!-- 中间 -->
            <div id="mid" :style="{'width':midwidth}">
                <div class="mid_title">
                    <ul>
                        <li class="hide_left" style="cursor:pointer;" @click="hideside('left')">
                            <img src="../../../static/imgs/left_hide.png" style="">
                        </li>
                        <li>{{coinitem.exchangeCode }}/{{coinitem.unitsCode }}</li>
                        <li>
                            <!-- ≈ ￥??? -->
                            <span style="font-size:15px;" class="depreciate-class"  :class="marketitem.diffRate>0?'hong':'lv'">{{marketitem.price||'----'}}</span></li> 
                        <li>涨幅
                            <span style="font-size:15px;" :class="marketitem.diffRate>0?'hong':'lv'" class="depreciate-class">{{marketitem.diffRate||'----'}}%</span>
                        </li>
                        <li>高 {{marketitem.high||'----'}}</li>
                        <li>低 {{ marketitem.low||'----'}}</li>
                        <!-- 24H量 ??? {{coinitem.exchangeCode }} -->
                        <li></li>
                        <li class="hide_right" @click="hideside('right')">
                            <img src="../../../static/imgs/right_hide.png" style="margin-top: 8px;" class="">
                        </li>
                    </ul>
                </div>

                <div class="kbox1">

                    <div id="k_line">
                        <div class="chat">
                            <div class="app-tv-chart-container" id="tv_chart_container" style="height:554px">
                                <trading-view height='540px' :code='coinitem.marketCode' :market='marketitem'></trading-view>
                            </div>
                        </div>
                        <!-- <div class="chart_toolbar_button yellow" id="chart_show_depth">深度图</div> -->

                        <!-- <div style="position: absolute; left: 20px; top: 40px; width: 96.5%; height: 514px; background: black; z-index: 120; display: block;">

                        </div> -->
                    </div>

                </div>
                <p class="mb_btn">
                    <button  :class="{'history-act':historyindex==0}" @click="historyindex=0">当前委托</button>
                    <button  :class="{'history-act':historyindex==1}" @click="historyindex=1">当前成交</button>
                    <button class="cd-popup-trigger3" :class="{'history-act':historyindex==2}" @click="historypop=true;historyindex=2">历史成交</button>
                </p>
                <!-- 历史记录弹出 -->
                <!-- <div class="cd-popup3 " :class="{'is-visible3':historypop}">
                    <div class="history-pop">
                        <div class="cd-popup-container3">
                            <a class="cd-popup-close" href="javascript:;" @click="historypop=false">×</a>
                            <div class="history-pop-header">
                                <div class="header-box">

                                </div>
                                <p class="companylogo">
                                    <a href="#/home">
                                        <img style="width:100px;" src="../../../static/imgs/logo.png"></a>
                                </p>
                            </div>

                            <div class="content-box tabPanel">
                                <ul class="history-tab">
                                    <li>历史成交</li>
                                </ul>
                            </div>
                            <div class="panes">
                                <div class="pane" style="display:none;">
                                    <div class="pane1">
                                        <table>
                                            <thead>
                                                <tr class="panes-thead">
                                                    <th>日期</th>
                                                    <th>交易对</th>
                                                    <th>方向</th>
                                                    <th>市价</th>
                                                    <th>数量</th>
                                                    <th>成交数量</th>
                                                    <th>撤单数量</th>
                                                    <th>金额</th>

                                                </tr>
                                            </thead>
                                            <tbody class="panes-body">
                                            </tbody>
                                        </table>
                                        <div class="nodata "  style="margin-top: 30px; display: block;text-align: center;">
                                            <img src="../../../static/imgs/record.png">
                                            <p class="">您暂无历史委托</p>
                                        </div>
                                    </div>

                                    <div class="pagenum">
                                        <ul class="pagination">
                                            <li>&lt;&lt;</li>
                                            <li>1</li>
                                            <li>2</li>
                                            <li>3</li>
                                            <li>4</li>
                                            <li>5</li>
                                            <li>&gt;&gt;</li>
                                        </ul>

                                    </div>
                                </div>

                                <div class="pane" style="display:block">
                                    <div class="pane" style="display:block;">
                                        <div class="pane1">
                                            <table>
                                                <thead>
                                                    <tr>
                                                        <th>日期</th>
                                                        <th>交易对</th>
                                                        <th>方向</th>
                                                        <th>单价</th>
                                                        <th>成交数量</th>
                                                        <th>手续费</th>
                                                        <th>金额</th>
                                                    </tr>
                                                </thead>
                                                <tbody>
                                                    <tr v-for="i in history" :key="i.id">
                                                        <td>{{i.createTime}}</td>
                                                        <td>交易对</td>
                                                        <td>方向</td>
                                                        <td>{{i.unitPrice}}</td>
                                                        <td>{{i.volumes}}</td>
                                                        <td>手续费</td>
                                                        <td>{{i.transactionPrice }}</td>
                                                    </tr>
                                                </tbody>
                                            </table>

                                            <div class="nodata ng-scope" ng-if="!trades.length" style="margin-top: 30px; display: block;text-align:center;" v-if="!history.length">
                                                <img src="../../../static/imgs/record.png">
                                                <p class="ng-binding">你没有历史成交记录</p>
                                            </div>
                                        </div>
                                        <div class="pagenum">
                                            <ul class="pagination">
                                                <li>&lt;&lt;</li>
                                                <li>1</li>
                                                <li>2</li>
                                                <li>3</li>
                                                <li>4</li>
                                                <li>5</li>
                                                <li>&gt;&gt;</li>
                                            </ul>

                                        </div>

                                    </div>
                                </div>

                            </div>

                            <em class="border-lefts"></em>
                            <em class="border-rights"></em>
                            <em class="border-bottoms"></em>
                            <em class="border-tops"></em>
                        </div>
                    </div>

                </div> -->
                <div class="mid_bottom">
                    <div class="mid_bottom_left">

                        <div class="login" v-if="!userinfo">
                            <p><img src="../../../static/imgs/smail.png"> 您还没有登录，请
                                <router-link to="/login">登录</router-link> 或
                                <router-link to="/register">注册</router-link> 后再尝试</p>
                        </div>

                        <div class="mid_bottom_data" v-if="userinfo">
                            <template v-if="historyindex==0">
                                <ul style="background: rgb(13, 40, 53); display: block;">
                                    <li >
                                        <span>日期</span>
                                        <span>交易对</span>
                                        <span>方向</span>
                                        <span>单价</span>
                                        <span>数量</span>
                                        <span>委托总额</span>
                                        <!-- <span>操作</span> -->
                                    </li>
                                </ul>
                                <ul style="height: 88%; display: block;">
                                    <li v-for="i in entrustlist" :key="i.id">
                                        <span>{{i.createTime}}</span>
                                        <span>{{i.symbol}}</span>
                                        <span>{{i.direction==0?'买入':'卖出'}}</span>
                                        <span>{{i.unitPrice}}</span>
                                        <span>{{i.remain}}</span>
                                        <span>{{i.orderPrice}}</span>
                                        <!-- <span>操作</span> -->
                                    </li>
                                    <Page :total="entrustTotal" size="small" :page-size='entrustrows' @on-change='pagechange' v-if="entrustTotal>entrustrows" />
                                </ul>
                                
                            </template>
                            <template v-if="historyindex==1">
                                <ul style="background: rgb(13, 40, 53);">
                                    <li >
                                        <span>日期</span>
                                        <span>交易对</span>
                                        <span>方向</span>
                                        <span>单价</span>
                                        <span>成交数量</span>
                                        <span>成交总额</span>
                                        <span>手续费</span>
                                    </li>
                                </ul>
                                <ul style="height: 88%;">
                                        <li v-for="i in entrustlist" :key="i.id">
                                            <span>{{i.createTime}}</span>
                                            <span>{{i.symbol}}</span>
                                            <span>{{i.direction==0?'买入':'卖出'}}</span>
                                            <span>{{i.unitPrice}}</span>
                                            <span>{{i.orderQuantity }}</span>
                                            <span>{{i.orderPrice}}</span>
                                            <span>{{i.fee}}</span>
                                        </li>
                                        <Page :total="entrustTotal" size="small" :page-size='entrustrows' @on-change='pagechange' v-if="entrustTotal>entrustrows" />
                                </ul>
                            </template>
                            <template v-if="historyindex==2">
                            <ul style="background: rgb(13, 40, 53);">
                                <li >
                                    <span>日期</span>
                                    <!-- <span>交易对</span> -->
                                    <!-- <span>方向</span> -->
                                    <span>市价</span>
                                    <span>数量</span>
                                    <span>成交数量</span>
                                    <!-- <span>撤单数量</span> -->
                                    <span>金额</span>                                      
                                </li>
                            </ul>
                            <ul style="height: 88%;">
                                <li v-for="i in history" :key="i.id">
                                    <span>{{i.createTime}}</span>
                                    <!-- <span>交易对</span> -->
                                    <!-- <span>方向</span> -->
                                    <span>{{i.unitPrice}}</span>
                                    <span>{{i.volumes}}</span>
                                    <!-- <span>手续费</span> -->
                                    <span>{{i.transactionPrice }}</span>
                                </li>
                                <Page :total="entrustTotal" size="small" :page-size='entrustrows' @on-change='pagechange' v-if="entrustTotal>entrustrows" />
                            </ul>
                            </template>
                        </div>
                    </div>

                    <div class="mid_bottom_right" style="margin-left:-16px;">
                        <div class="buy" style="margin-left:-30px;">
                            <div class="buy-box">
                                <span class="order_type_radio">
                                    <div class="ant-radio-group">
                                        <label class="ant-radio-wrapper" @click="buy.orderType=1">
                                            <span class="ant-radio" :class="{'ant-radio-checked':buy.orderType}">
                                                <input checked="" class="ant-radio-input" type="radio">
                                                <span class="ant-radio-inner"></span>
                                            </span>
                                            <span style="color: rgb(122,151,240);">限价</span>
                                        </label>
                                        <label class="ant-radio-wrapper" @click="buy.orderType=0">
                                            <span class="ant-radio" :class="{'ant-radio-checked':!buy.orderType}">
                                                <input class="ant-radio-input" type="radio">
                                                <span class="ant-radio-inner"></span>
                                            </span>
                                            <span style="color: rgb(122,151,240);">市价</span>
                                        </label>
                                    </div>
                                </span>
                                <div class="buy_one" id="orderBuy">
                                    <p>买入:&nbsp;
                                        <span>{{coinitem.exchangeCode }}</span>
                                    </p>
                                    <p :title="coinitem.unitsCode">可用:&nbsp;{{coinitem.unitsCode }}</p>
                                    <p>
                                        <template v-if="buy.orderType==0">
                                            <span>以行情最优价买入</span>
                                        </template>
                                        <template v-else>
                                            <span>买入价</span><input v-model="buy.unitPrice">
                                        </template>
                                        
                                        <span>{{coinitem.unitsCode }}</span>
                                    </p>
                                    <p>
                                        <span>买入量</span><input v-model="buy.orderQuantity">
                                        <span>{{coinitem.exchangeCode }}</span>
                                    </p>
                                    <p>
                                        <span class="border-bg"></span>
                                        <span></span>
                                        <span></span>
                                        <span></span>
                                        <span></span>
                                    </p>

                                    <p >预计交易额:&nbsp;{{orderbuy}}&nbsp;{{coinitem.unitsCode }}</p>
                                    <p>
                                        <button style="background: #19b34c;" @click="order(buy)">立即买入</button>
                                    </p>
                                </div>
                            </div>
                        </div>

                        <div class="buy">
                            <div class="buy-box">
                                <span class="order_type_radio">
                                    <div class="ant-radio-group">
                                        <label class="ant-radio-wrapper" @click="sell.orderType=1">
                                            <span class="ant-radio " :class="{'ant-radio-checked':sell.orderType}">
                                                <input checked="" class="ant-radio-input" type="radio">
                                                <span class="ant-radio-inner"></span>
                                            </span>
                                            <span style="color: rgb(122,151,240);">限价</span>
                                        </label>
                                        <label class="ant-radio-wrapper"  @click="sell.orderType=0">
                                            <span class="ant-radio" :class="{'ant-radio-checked':!sell.orderType}">
                                                <input class="ant-radio-input" type="radio">
                                                <span class="ant-radio-inner"></span>
                                            </span>
                                            <span style="color: rgb(122,151,240);">市价</span>
                                        </label>
                                    </div>
                                </span>
                                <div class="buy_one" id="sale">
                                    <p>卖出:&nbsp;
                                        <span>{{coinitem.exchangeCode }}</span>
                                    </p>
                                    <p :title="coinitem.exchangeCode">可用:&nbsp;{{coinitem.exchangeCode }}</p>
                                    <p>
                                        <template v-if="sell.orderType==0">
                                            <span>以行情最优价卖出</span>
                                        </template>
                                        <template v-else>
                                            <span>卖出价</span><input v-model="sell.unitPrice">
                                        </template>

                                        <!-- <span>卖出价</span><input v-model="sell.unitPrice"> -->
                                        <span>{{coinitem.unitsCode }}</span>
                                    </p>
                                    <p>
                                        <span>卖出量</span><input v-model="sell.orderQuantity">
                                        <span>{{coinitem.exchangeCode }}</span>
                                    </p>
                                    <p>
                                        <span class="border-bg"></span>
                                        <span></span>
                                        <span ></span>
                                        <span></span>
                                        <span></span>
                                    </p>
                                    <p>预计交易额:&nbsp;{{ordersell}}&nbsp;{{coinitem.unitsCode }}</p>
                                    <p>
                                        <button  @click="order(sell)">立即卖出</button>
                                    </p>
                                </div>
                            </div>

                        </div>
                    </div>
                </div>
            </div>
            <!-- 右边 -->
            <div id="right_panel" v-show="!hideright">
                <div class="right_title">
                    <!-- <ul>
                        <li class="active"></li>
                        <li></li>
                        <li></li>
                        <li>深度：</li>
                        <li>
                            <select class="r_select">
                                <option>8位小数</option>
                            </select>
                        </li>
                    </ul> -->
                </div>
                <div class="price">
                    <div class="price1">
                        <p>
                            <span>价格({{coinitem.unitsCode }})</span>
                            <span>数量({{coinitem.exchangeCode }})</span>
                            <span>总额({{coinitem.unitsCode}})</span>
                        </p>
                        <div class="price1_top">
                            <ul class="askTable">
                                <li class="" v-for="i in 5" :key="i">
                                    <span>{{marketitem[`bp${i}`]||0}}</span>
                                    <span>{{marketitem[`bv${i}`]||0}}</span>
                                    <span>{{(marketitem[`bp${i}`]*marketitem[`bv${i}`]).toFixed(8)}}</span>
                                    <span style="width: 4.51893px;"></span>
                                </li>
                            </ul>
                        </div>
                        <p class="price1_mid">
                            <span class="price-depreciate">{{marketitem.price}}</span>
                            <!-- ￥43011.15 -->
                            <span></span>

                        </p>
                        <ul class="price1_bottom">
                            <li class="" v-for="i in 5" :key="i">
                                <span>{{marketitem[`sp${i}`]||0}}</span>
                                <span>{{marketitem[`sv${i}`]||0}}</span>
                                <span> {{(marketitem[`sp${i}`]*marketitem[`sv${i}`]).toFixed(8)}}</span>
                                <!-- <span style="width: 191.927px;"></span> -->
                            </li>
                        </ul>

                    </div>
                </div>
                <div class="order">
                    <p>实时成交</p>
                    <div class="order1">
                        <p>
                            <span>价格({{coinitem.unitsCode }})</span>
                            <span>数量({{coinitem.exchangeCode }})</span>
                            <span>时间</span>
                        </p>
                        <ul>
                            <li class="" v-for="i in latestdeal" :key="i.id">
                                <span>{{i.transactionPrice }}</span>
                                <span>{{i.volumes}}</span>
                                <span>{{i.createTime.split(' ')[1]}}</span>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script type="">
import tradeheader from "../common/tradeheader";
import tradingView from "../tradingView/index";
export default {
    name: "coin",
    data() {
        return {
            userinfo:this.$cookie.get('_auth'),
            category: [], //币种列表
            coinitem: {}, //默认币种
            markettimer:null,//行情定时器
            market:[],//行情列表
            marketitem:{},//当前币种行情
            midwidth:'71.5%',//中间宽度
            hideright:false,//隐藏左边或右边侧栏
            hideleft:false,
            buy:{
                // categoryId:this.coinitem.id,//币币交易种类ID
                direction:0,//方向 0:买入 1:卖出
                orderType:1,//交易类型 0:市价 1限价
                unitPrice:'',//单价
                orderQuantity:'',//交易数量
            },
            sell:{
                direction:1,//方向 0:买入 1:卖出
                orderType:1,//交易类型 0:市价 1限价
                unitPrice:'',//单价
                orderQuantity:'',//交易数量
            },
            historypop:false,//历史订单显示
            historyindex:0,//委托选择
            entrustlist:[],//委托列表
            entrustTotal:0,//委托总数
            entrustrows:10,//委托条数
            entruststart:0,//从哪开始
            latestdeal:[],//最新成交
            history:[],//历史成交
            path:process.env.LIBRARYPATH
        };
    },
    components: {
        tradeheader,tradingView
    },
    destroyed () {
        if (this.markettimer) {
            clearInterval(this.markettimer)
            this.markettimer=null
        }
    },
    created() {
        this.getcategory();
        this.getentrust()
        this.getlatestdeal()
        this.gethistory()
        
    },
    watch: {
        'sell.ordertype'(v){//交易类型变化时
        // 交易类型为
          if (v==0) {
            this.orderprice=this.market.price
          }else{
            this.orderprice=''
          }
        },
        'buy.ordertype'(v){//交易类型变化时
        // 交易类型为
          if (v==0) {
            this.orderprice=this.market.price
          }else{
            this.orderprice=''
          }
        },
        'coinitem.marketCode'(){//当前货币code
            this.market.forEach((i,index)=>{
                if (i.code==this.coinitem.marketCode){
                    this.marketitem=i
                }
            })
            this.getentrust()
        },
        historyindex(){//当前委托 历史委托切换时重新
            this.getentrust()
        }
    },
    computed: {
        orderbuy(){//交易额
            return (+this.buy.unitPrice*+this.buy.orderQuantity).toFixed(4)
        },
        ordersell(){//交易额
            return (this.sell.unitPrice*this.sell.orderQuantity).toFixed(4)
        }
    },
    methods: {
        //获取币种
        getcategory() {
            this.$axios
                .post("api/trade/coin/category")
                .then(res => {
                    if (!res.data.errorCode) {
                        this.category = res.data.data;
                        this.coinitem = res.data.data[0];
                        this.setmarkettimer();
                        // this.getmarket()
                        // this.initkline();
                    }
                })
                .catch(err => {});
        },
        //定时获取行情
        setmarkettimer() {
            
            if (this.markettimer) {
                clearInterval(this.markettimer);
                this.markettimer=null
            }else{
              this.getmarket()
            }
            this.markettimer = setInterval(() => {
              this.getmarket()
            }, 3000);
        },
        //获取行情
        getmarket() {
            var arr=[]
            this.category.forEach(function(i,index){
                // console.log(i.marketCode);
                arr.push(i.marketCode)
            })
            console.log(arr.join(','));
            // console.log(this.category.marketCode);
            this.$axios.get(`/api/market/latest_market?codes=${arr.join(',')}`).then(res => {
                if (!res.data.errorCode) {
                    // console.log(res.data.data);
                    this.market = res.data.data;
                    this.market.forEach((i,index) => {//循环行情列表
                        // console.log(i.diff,i.diffRate);
                        if (i.code==this.coinitem.marketCode){
                            this.marketitem=i
                        }
                        this.category.forEach((j,index1) => {//循环货币列表
                            if (i.code==j.marketCode) {
                                // console.log(i.code,j.marketCode);  
                                this.$set(this.category[index1],'diff',i.diff)
                                this.$set(this.category[index1],'diffRate',i.diffRate)
                            }
                        });
                    });
                }
            }).catch(err => {});
        },
        // 隐藏侧边栏
        hideside(direction){
            if (direction=='left') {
                if (this.hideleft) {
                    this.hideleft=false
                    if (this.hideright==true) {
                        this.midwidth='86%'
                        return
                    }
                    this.midwidth='71.5%'
                    return
                }
                if (this.hideright==true) {
                    this.midwidth='100%'
                    this.hideleft=true
                    return
                }
                this.hideleft=true
                this.midwidth='86%'
            }else if(direction=='right'){
                if (this.hideright) {
                    this.hideright=false
                    if (this.hideleft==true) {
                        this.midwidth='86%'
                        return
                    }
                    this.midwidth='71.5%'
                    return
                }
                if (this.hideleft==true) {
                    this.midwidth='100%'
                    this.hideright=true
                    return
                }
                this.hideright=true
                this.midwidth='86%'

            }
        },
        //下单
        order(params){
            if (!params.unitPrice) {
                this.$Message.warning('请输入金额')
                return
            }
            // if (params.unitPrice=='') {
            //     params.unitPrice=undefined
            // }
            if (!params.orderQuantity) {
                this.$Message.warning('请输入数量')
                return
            }
            params.categoryId=this.coinitem.id
             console.log(params);
            this.$axios({
                url:'api/trade/coin/place_order',
                method:'post',
                headers:{
                'Content-Type':'application/json;charset=utf-8'
                },
                params
            }).then(res=>{
                if (!res.data.errorCode) {
                    this.$Message.success('委托成功!')
                    params.unitPrice=''
                    params.orderQuantity=''
                    this.getentrust()
                }
            }).catch(err=>{

            })
        },
        //查询交易委托
        getentrust(){
         if (this.$cookie.get('_auth')) {
             this.$axios.post('api/trade/coin/query_entrust',{
                start:this.entruststart,
                rows:this.entrustrows,
                status:this.historyindex
            }).then(res=>{
                if (!res.data.errorCode) {
                    this.entrustlist=res.data.page.data
                    this.entrustTotal=res.data.page.total
                }
            }).catch(err=>{

            })
        }
           
        },
        //获取最新交易
        getlatestdeal(){
            this.$axios.post('api/trade/coin/latest_deal').then(res=>{
                if (!res.data.errorCode) {
                    this.latestdeal=res.data.data
                }
                console.log(this.latestdeal);
                
            }).catch(err=>{

            })
        },
        //获取历史交易
        gethistory(){
            if (!this.$cookie.get('_auth')) {
                return
            }
            this.$axios.post('api/trade/coin/history').then(res=>{
                if (!res.data.errorCode) {
                    this.history=res.data.page.data
                }
            })
        },
        //选择价格
        chooseprice(type,price,num){
          if (this.trantabactive==type) {
            this.orderprice=price
            this.ordernum=num
          }
        },
        //翻页
        pagechange(index){
            console.log(index);
            this.entrustlist=[]
            this.entruststart=this.entrustrows*(index-1)
            this.getentrust()
            
        },


    }
};
</script>

<style scoped lang="less">
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: "\5FAE\8F6F\96C5\9ED1";
    color: white;
}
img {
    vertical-align: middle;
    border-style: none;
}
button {
    outline: none;
    border: none;
}
.trade_main {
    width: 100%;
    margin: 0 auto;
    overflow: hidden;
    min-width: 1813px;
    min-height: 990px;
    height: 943px;
    background: #000;
}
.history-act{
    background-color: rgb(43, 68, 97) !important;
}
// 主体内容
#trade_body {
    width: 100%;
    height: calc(100% - 45px);
    > div {
        float: left;
    }
    // 左边内容
    #left {
        width: 14%;
        height: 100%;
        margin-left: 0px;
        position: relative;
        z-index: 222;
        .sorts {
            margin-left: 10px;
            height: 104px;
            width: 100%;
            border: 26px solid transparent;
            border-image: url(../../../static/imgs/border.png) 30 30 round;
            .sorts_top {
                width: 100%;
                margin-top: -10px;
                padding-left: 12px;
                li {
                    color: white;
                    font-size: 12px;
                    float: left;
                    width: 24%;
                    height: 24px;
                    line-height: 24px;
                    text-align: center;
                    background: linear-gradient(
                        180deg,
                        #01080b 0,
                        #082d3e 28%,
                        #09374c 50%,
                        #082d3e 80%,
                        #01080b
                    );
                    span {
                        display: inline-block;
                        background: #000;
                        width: calc(100% - 2px);
                        cursor: pointer;
                        color: white;
                        font-size: 12px;
                        height: 100%;
                    }
                }
                .sorts-active > span {
                    color: #f4f100 !important;
                }
            }
            .sorts-line {
                width: 100%;
                height: 1px;
                background: linear-gradient(
                    to right,
                    rgba(0, 0, 0, 0) 0%,
                    #09374c 10%,
                    #126b95 20%,
                    #1787bc 30%,
                    #1fb4fb 40%,
                    #1eaff4 50%,
                    #1eaff4 60%,
                    #1787bc 70%,
                    #126b95 80%,
                    #09374c 90%,
                    rgba(0, 0, 0, 0) 100%
                );
                margin-top: 30px;
                margin-left: -10px;
            }
            .search {
                width: 100%;
                height: 44px;
                line-height: 44px;
                color: white;
                margin-top: 6px;
                p {
                    width: calc(100% - 54px);
                }
                p:first-child {
                    float: left;
                    height: 26px;
                    line-height: 44px;
                    background: url(../../../static/imgs/search.png) no-repeat
                        100% 2px;
                    background-color: #062432;
                    border-radius: 13px;
                    margin-top: 8px;
                    input {
                        width: calc(100% - 50px);
                        border: 0px;
                        margin-left: 20px;
                        margin-top: 4px;
                        float: left;
                        background-color: #062432;
                        outline: none;
                        height: 20px;
                    }
                }
                p:last-child {
                    width: 54px;
                    float: right;
                    font-size: 12px;
                    line-height: 44px;
                    text-align: right;
                    background: url(../../../static/imgs/transform.png)
                        no-repeat 2px 10px;
                }
            }
        }
        .main_area {
            height: calc((100% - 71px));
            width: 100%;
            margin-top: 42px;
            .data {
                width: 100%;
                box-sizing: border-box;
                height: calc(100% - 38px) !important;
                border: 26px solid transparent;
                border-image: url(../../../static/imgs/border.png) 30 30 round;
                margin-left: 10px;
                .sorts_bottom button {
                    width: 25%;
                    height: 20px;
                    color: white;
                    float: left;
                    font-size: 9px;
                    border-radius: 5px;
                    margin-left: 8px;
                    margin-top: -14px;
                    background-color: #062432;
                }
                .sorts_bottom button:first-child {
                    text-align: left;
                    padding-left: 10px;
                    background: url();
                    background-color: #062432;
                }
                .left-line {
                    width: 100%;
                    height: 1px;
                    background: linear-gradient(
                        to right,
                        rgba(0, 0, 0, 0) 0%,
                        rgba(0, 0, 0, 0) 10%,
                        #09374c 20%,
                        #126b95 30%,
                        #1787bc 40%,
                        #1fb4fb 50%,
                        #1eaff4 60%,
                        #1787bc 70%,
                        #126b95 80%,
                        rgba(0, 0, 0, 0) 90%,
                        rgba(0, 0, 0, 0) 100%
                    );
                }
                ul li {
                    width: 100%;
                    z-index: 100;
                }
                .list-left {
                    padding: 20px 0;
                    margin: 0;
                    display: -webkit-box;
                    display: -ms-flexbox;
                    display: flex;
                    -ms-flex-pack: distribute;
                    justify-content: space-around;
                    .depreciate-class {
                        font: 16px "\5FAE\8F6F\96C5\9ED1";
                        color: #fc5a5d;
                        border-radius: 2px;
                    }
                }
                .maketlist {
                    width: 100%;
                    height: 100%;
                    overflow-x: hidden;
                    overflow-y: auto;
                    > li {
                        cursor: pointer;
                        .list-left {
                            > img {
                                height: 16px;
                                margin-top: 30px;
                            }
                            .list-icon {
                                text-align: center;
                                width: 90px;
                                > img {
                                    margin: 5px auto;
                                    width: 50px;
                                }
                                > h6 {
                                    font-size: 15px;
                                    margin-left: -2px;
                                    font-weight: normal;
                                }
                            }
                            .list-left-right {
                                width: 90px;
                                > h2 {
                                    margin-top: 11px;
                                }
                                > p {
                                    font-size: 12px;
                                    margin-top: 7px;
                                }
                            }
                        }
                    }
                }
                .maketlist-left-bg {
                    background: linear-gradient(
                        to right,
                        rgba(0, 0, 0, 0) 0%,
                        rgba(0, 0, 0, 0) 10%,
                        rgba(6, 24, 49, 0.6) 20%,
                        rgba(8, 44, 63, 0.6) 30%,
                        rgba(9, 50, 70, 0.6) 40%,
                        rgba(9, 50, 70, 0.6) 50%,
                        rgba(9, 50, 70, 0.6) 60%,
                        rgba(8, 44, 63, 0.6) 70%,
                        rgba(6, 24, 49, 0.6) 80%,
                        rgba(0, 0, 0, 0) 90%,
                        rgba(0, 0, 0, 0) 100%
                    ) !important;
                    z-index: 1000;
                }
            }
        }
    }
    // 中间
    #mid {
        width: 71.5%;
        height: 100%;
        margin-left: 0px;
        .mid_title {
            width: 100%;
            height: 44px;
            > ul {
                width: 97%;
                height: 44px;
                margin-left: 16px;
                li {
                    float: left;
                    color: white;
                    font-size: 15px;
                    line-height: 44px;
                }
                li:nth-child(1) {
                    margin-left: 10px;
                }
                li:nth-child(2) {
                    margin-left: 28px;
                }
                li:nth-child(3) {
                    margin-left: 62px;
                }
                li:nth-child(4) {
                    margin-left: 46px;
                }
                li:nth-child(5) {
                    margin-left: 26px;
                }
                li:nth-child(6) {
                    margin-left: 26px;
                }
                li:nth-child(7) {
                    margin-left: 36px;
                }
                li:nth-child(8) {
                    float: right;
                }
            }
        }
        .kbox1 {
            width: 100%;
            height: calc((100% - 64px) * 0.65);
            position: relative;
            #k_line {
                width: 97.5%;
                height: 100%;
                margin-left: 16px;
                border: 26px solid transparent;
                border-image: url(../../../static/imgs/border.png) 30 30 round;
                .chat[_ngcontent-c4] {
                    width: calc((100% + 34px));
                    height: calc((100% + 34px));
                    position: relative;
                    margin-left: -20px;
                }
                .app-tv-chart-container[_ngcontent-c4] {
                    height: 100%;
                    margin-top: -20px;
                }
                .chart_toolbar_button {
                    position: absolute;
                    top: 18px;
                    right: 0;
                    margin-right: 90px;
                    font-size: 14px;
                    color: #a2a2a2;
                    cursor: pointer;
                }
            }
        }
        .mb_btn {
            width: 100%;
            height: 24px;
            margin-left: 10px;
            font-size: 12px;
            button {
                float: left;
                width: 124px;
                height: 100%;
                color: white;
                border-radius: 2px;
                margin-left: 10px;
                background-color: #062432;
            }
        }
        // 弹出历史记录
        .cd-popup3 {
            position: fixed;
            left: 0;
            top: 0;
            height: 100%;
            width: 100%;
            background-color: hsla(0, 0%, 56%, 0.6);
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.3s 0s, visibility 0s 0.3s;
            z-index: 9999;
            .history-pop {
                width: 1000px;
                margin: 50px auto;
                height: 625px;
                border: 1px solid #0a3b4c;
                padding-bottom: 10px;
                padding-right: 10px;
                .cd-popup-container3 {
                    position: relative;
                    top: -6px;
                    right: 6px;
                    width: 1000px;
                    height: 625px;
                    background: #000;
                    box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
                    border: 1px solid #0a3b4c;
                    -webkit-transform: scale(0.8);
                    transform: scale(0.8);
                    -webkit-backface-visibility: hidden;
                    transition-property: transform;
                    transition-duration: 0.3s;
                    .cd-popup-close {
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
                    .history-pop-header {
                        width: 100%;
                        padding: 15px 0;
                        display: flex;
                        justify-content: space-between;
                        .header-box {
                            padding-left: 50px;
                            .loginname img {
                                width: 14px;
                            }
                        }
                        .companylogo {
                            padding-right: 50px;
                        }
                    }
                    .content-box {
                        height: 34px;
                        background: #062432;
                        padding: 3px 30px;
                        box-sizing: border-box;
                        .history-tab {
                            text-align: left;
                            display: inline-block;
                            border: 1px solid #0a0204;
                            border-radius: 4px;
                            overflow: hidden;
                            li {
                                float: left;
                                width: 84px;
                                height: 25px;
                                line-height: 25px;
                                text-align: center;
                                font-size: 14px;
                                cursor: pointer;
                            }
                            .tab-active {
                                background: #2b4461;
                            }
                        }
                    }
                    .panes {
                        padding: 12px 16px;
                        height: 530px;
                        .pane {
                            height: 530px;
                        }
                        .pane1 {
                            height: 480px;
                            overflow-y: hidden;
                        }
                        table {
                            width: 100%;
                            border-collapse: collapse;
                            border-spacing: 0;
                            th {
                                text-align: center;
                                font-size: 13px;
                                color: #1986d9;
                                height: 25px;
                                line-height: 25px;
                                border-bottom: 1px solid #083347;
                            }
                            td{
                                text-align: center;
                                font-size: 13px;
                                line-height: 30px;
                                // margin-top: 8px;
                            }
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
                        .pagenum {
                            width: 100%;
                            height: 30px;
                            .pagination {
                                padding-left: 0;
                                border-radius: 4px;
                                display: block;
                                height: 100%;
                                width: 335px;
                                margin: 0 auto;
                                li {
                                    display: inline-block;
                                    width: 40px;
                                    height: 100%;
                                    border: 1px solid #999;
                                    border-radius: 2px;
                                    text-align: center;
                                    line-height: 28px;
                                    margin-left: 3px;
                                }
                            }
                        }
                    }
                    .border-lefts,
                    .border-rights,
                    .border-bottoms,
                    .border-tops {
                        display: inline-block;
                        width: 10px;
                        height: 10px;
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
                }
            }
        }
        .cd-popup3.is-visible3 {
            opacity: 1;
            visibility: visible;
            transition: opacity 0.3s 0s, visibility 0s 0s;
            .cd-popup-container3 {
                transform: scale(1);
            }
        }
        .mid_bottom {
            width: 100%;
            height: calc((100% - 76px) * 0.35);
            .mid_bottom_left {
                width: 64%;
                height: 100%;
                margin-left: 16px;
                float: left;
                position: relative;
                border: 26px solid transparent;
                border-image: url(../../../static/imgs/border.png) 30 30 round;
                .login {
                    margin-top: 2px;
                    width: 100%;
                    height: calc(100% - 24px + 18px);
                    border: 1px solid #062432;
                    p {
                        height: 100%;
                        line-height: 24px;
                        text-align: center;
                        color: white;
                        font-size: 12px;
                        margin-top: 60px;
                        a {
                            color: #1daaec;
                            font-size: 12px;
                        }
                    }
                }
                .mid_bottom_data {
                    width: 100%;
                    height: 100%;
                    ul {
                        width: 100%;
                        overflow-x: hidden;
                        overflow-y: auto;
                       text-align: center;
                        li {
                            width: 100%;
                            height: 28px;
                            // line-height: 28px;
                             display: flex;
                            span {
                                flex: 1;
                                float: left;
                                text-align: center;
                                color: white;
                                font-size: 12px;
                                margin-top: 8px;
                            }
                        }
                    }
                }
            }
            .mid_bottom_right {
                width: 32%;
                height: 100%;
                float: right;
                margin-right: 0px;
                .buy {
                    width: 50%;
                    height: 100%;
                    margin-left: 10px;
                    float: left;
                    position: relative;
                    border: 25px solid transparent;
                    border-image: url(../../../static/imgs/border.png) 30 30
                        round;
                    .order_type_radio {
                        height: 0.00001rem;
                        color: rgb(122, 151, 240);
                        font-size: 6px;
                        margin-top: -20px;
                        .ant-radio-group {
                            display: inline-block;
                            font-size: 12px;
                            .ant-radio-wrapper {
                                font-size: 12px;
                                display: inline-block;
                                position: relative;
                                white-space: nowrap;
                                margin-right: 8px;
                                cursor: pointer;
                                float: left;
                                .ant-radio {
                                    white-space: nowrap;
                                    outline: none;
                                    display: inline-block;
                                    position: relative;
                                    line-height: 1;
                                    vertical-align: text-bottom;
                                    cursor: pointer;
                                    input {
                                        position: absolute;
                                        left: 0;
                                        z-index: 1;
                                        cursor: pointer;
                                        opacity: 0;
                                        top: 0;
                                        bottom: 0;
                                        right: 0;
                                    }
                                    .ant-radio-inner {
                                        position: relative;
                                        top: 0;
                                        left: 0;
                                        display: block;
                                        width: 14px;
                                        height: 14px;
                                        border-width: 1px;
                                        border-style: solid;
                                        border-radius: 14px;
                                        border-color: #108ee9;
                                        transition: all 0.3s;
                                    }
                                    .ant-radio-inner:after {
                                        position: absolute;
                                        width: 6px;
                                        height: 6px;
                                        left: 3px;
                                        top: 3px;
                                        border-radius: 4px;
                                        display: table;
                                        border-top: 0;
                                        border-left: 0;
                                        content: " ";
                                        background-color: #108ee9;
                                        opacity: 0;
                                        transform: scale(0);
                                        transition: all 0.3s
                                            cubic-bezier(0.78, 0.14, 0.15, 0.86);
                                    }
                                }
                                .ant-radio-checked:after {
                                    position: absolute;
                                    top: 0;
                                    left: 0;
                                    width: 100%;
                                    height: 100%;
                                    border-radius: 50%;
                                    border: 1px solid #108ee9;
                                    content: "";
                                    animation: antRadioEffect 0.36s ease-in-out;
                                    -webkit-animation-fill-mode: both;
                                    animation-fill-mode: both;
                                    visibility: hidden;
                                }
                                .ant-radio-checked .ant-radio-inner:after {
                                    transform: scale(1);
                                    opacity: 1;
                                    transition: all 0.3s
                                        cubic-bezier(0.78, 0.14, 0.15, 0.86);
                                }
                                span.ant-radio + * {
                                    padding-left: 8px;
                                    padding-right: 8px;
                                }
                            }
                        }
                    }
                    @-webkit-keyframes antRadioEffect {
                        0% {
                            -webkit-transform: scale(1);
                            transform: scale(1);
                            opacity: 0.5;
                        }
                        100% {
                            -webkit-transform: scale(1.6);
                            transform: scale(1.6);
                            opacity: 0;
                        }
                    }
                    .buy_one {
                        width: calc(100% + 18px);
                        height: calc(76%);
                        margin-left: -10px;
                        margin-top: 5px;
                        p:nth-child(1) {
                            width: 100%;
                            height: 12%;
                            margin-left: 10px;
                            line-height: 24px;
                            font-size: 12px;
                            span {
                                line-height: 22px;
                            }
                        }
                        p:nth-child(2) {
                            width: 184px;
                            height: 12%;
                            margin-left: 10px;
                            line-height: 24px;
                            font-size: 12px;
                            white-space: nowrap;
                            text-overflow: ellipsis;
                            overflow: hidden;
                        }
                        p:nth-child(3),
                        p:nth-child(4) {
                            height: 2rem;
                            background-color: #062432;
                            border-radius: 5px;
                            margin: 0 auto;
                            margin-top: 4px;
                            line-height: 2rem;
                            font-size: 0.1rem;
                            span {
                                float: left;
                            }
                            span:first-child {
                                margin-left: 10px;
                            }
                            input {
                                border: 0px;
                                width: calc(100% - 106px);
                                margin-left: 4px;
                                margin-top: 6px;
                                float: left;
                                background-color: #062432;
                                outline: none;
                                height: 20px;
                                text-align: right;
                            }
                            span:last-child {
                                margin-left: 5px;
                            }
                        }
                        p:nth-child(5) {
                            height: 0.2rem;
                            margin: 20px auto;
                            position: relative;
                            span {
                                width: 8px;
                                height: 8px;
                                background-color: #1fb5fc;
                                border-radius: 4px;
                                position: absolute;
                                top: 2px;
                            }
                            span:nth-child(2) {
                                margin-left: 35%;
                            }
                            span:nth-child(1) {
                                margin-left: 13%;
                                font-size: 0.0001rem;
                            }
                            span:nth-child(3) {
                                margin-left: 59%;
                            }
                            span:nth-child(4) {
                                margin-left: 82%;
                            }
                            span:nth-child(5) {
                                width: 64%;
                                height: 2px;
                                top: 5px;
                                margin-left: 18%;
                            }
                            .border-bg {
                                border: 1px solid #157cac;
                                background-color: black !important;
                                z-index: 100;
                            }
                        }
                        p:nth-child(6) {
                            width: 184px;
                            height: 8%;
                            line-height: 24px;
                            margin-left: 4%;
                            font-size: 12px;
                            margin-top: 6px;
                            white-space: nowrap;
                            text-overflow: ellipsis;
                            overflow: hidden;
                        }
                        p:nth-child(7) {
                            width: 100%;
                            height: 16%;
                            text-align: center;
                            line-height: 30px;
                            margin-top: 12px;
                            button {
                                width: 80%;
                                height: 2rem;
                                background-color: #f20123;
                                border-radius: 15px;
                            }
                        }
                    }
                }
            }
        }
    }
    // 右边
    #right_panel {
        float: right;
        width: 14%;
        height: 100%;
        margin-left: 0px;
        margin-right: 0px;
        .right_title {
            width: 100%;
            height: 28px;
            ul {
                width: 100%;
                height: 100%;
                li {
                    float: left;
                    width: 16%;
                    height: 100%;
                    margin-top: 5px;
                    font-size: 12px;
                }
                li:nth-child(1) {
                    background: url(../../../static/imgs/de1.png) no-repeat 5px
                        6px;
                    border: 1px solid #1987da;
                }
                li:nth-child(2) {
                    background: url(../../../static/imgs/de2.png) no-repeat;
                    border: 1px solid #1987da;
                    height: 28px;
                    margin-left: 4px;
                }
                li:nth-child(3) {
                    background: url(../../../static/imgs/de3.png) no-repeat;
                    border: 1px solid #1987da;
                    height: 28px;
                    margin-left: 4px;
                }
                li:nth-child(4) {
                    margin-top: 12px;
                    text-align: right;
                }
                li:last-child {
                    margin-top: 8px;
                    width: 32%;
                    height: 24px;
                    float: right;
                    font-size: 12px !important;
                    .r_select {
                        max-width: 72px;
                        height: 100%;
                        background-color: #062432;
                        border-radius: 5px;
                        float: right;
                        border: none;
                        margin-right: 10px;
                        option {
                            font-size: 12px !important;
                        }
                    }
                }
                .active {
                    border: 1px solid #cbc800 !important;
                }
            }
        }
        .price {
            width: calc(100% + 12px);
            height: calc((100% - 18px) * 0.55);
            min-width: 259px;
            position: relative;
            top: 16px;
            right: 20px;
            border: 26px solid transparent;
            border-image: url(../../../static/imgs/border.png) 30 30 round;
            .price1 {
                width: calc(100% + 52px);
                height: calc(100% + 32px);
                overflow: hidden;
                margin-left: -22px;
                min-width: 259px;
                margin-top: -20px;
                > p:first-child {
                    width: 95%;
                    height: 16px;
                    font-size: 10px;
                    margin-left: 10px;
                    margin-bottom: 6px;
                    span {
                        line-height: 20px;
                        font-size: 12px;
                        float: left;
                    }
                    span:nth-child(1) {
                        width: 26%;
                        text-align: left;
                    }
                    span:nth-child(2) {
                        width: 25%;
                        text-align: right;
                    }
                    span:nth-child(3) {
                        width: 42%;
                        text-align: right;
                    }
                }
                .price1_top {
                    height: calc((100% - 16px - 26px) / 2 - 16px);
                    overflow: hidden;
                    width: 95%;
                    position: relative;
                    margin-left: 10px;
                    overflow-anchor: none;
                    .askTable {
                        position: absolute;
                        // bottom: 0;
                        left: 0;
                        width: 100%;
                    }
                    > ul:nth-child(1) {
                        width: 100%;
                        overflow: hidden;
                        li {
                            width: 100%;
                            height: 17px;
                            position: relative;
                            line-height: 17px;
                            span {
                                line-height: 17px;
                                font-size: 12px;
                                position: relative;
                                z-index: 3;
                                float: left;
                            }
                            span:nth-child(1) {
                                width: 25%;
                                text-align: left;
                                color: #f20123;
                            }
                            span:nth-child(2) {
                                width: 25%;
                                text-align: right;
                                margin-right: 6px;
                            }
                            span:nth-child(3) {
                                width: 40.5%;
                                text-align: right;
                            }
                            span:last-child {
                                height: 100%;
                                position: absolute;
                                top: 0px;
                                right: 30px;
                                background-color: #2d0008;
                                z-index: 2;
                            }
                        }
                    }
                }
                .price1_mid {
                    height: calc((100% - 156px) * 0.1);
                    width: 270px;
                    background: rgb(38, 45, 51, 0.4);
                    line-height: 30px !important;
                    margin-left: -10px;
                }
                > p:nth-child(3) {
                    width: 100%;
                    height: 26x;
                    color: #18a949;
                    span {
                        line-height: 30px;
                        font-size: 14px;
                        float: left;
                    }
                    span:nth-child(1) {
                        width: 50%;
                        text-align: right;
                        padding-right: 26px;
                    }
                    span:last-child {
                        color: #f4f100;
                        width: 33%;
                        text-align: right;
                    }
                }
                .price1_bottom {
                    height: calc((100% - 16px - 40px) / 2);
                    margin-left: 10px;
                    margin-bottom: 15px;
                }
                > ul:nth-child(4) {
                    width: 100%;
                    overflow: hidden;
                    li {
                        position: relative;
                        width: 100%;
                        height: 17px;
                        margin-top: 1px;
                        line-height: 17px;
                        span {
                            line-height: 17px;
                            font-size: 12px;
                            position: relative;
                            z-index: 3;
                            float: left;
                        }
                        span:nth-child(1) {
                            width: 25%;
                            text-align: left;
                            color: #19b34c;
                        }
                        span:nth-child(2) {
                            width: 25%;
                            text-align: right;
                            margin-right: 6px;
                        }
                        span:nth-child(3) {
                            width: 40.5%;
                            text-align: right;
                        }
                        span:last-child {
                            height: 100%;
                            position: absolute;
                            top: 0px;
                            right: 30px;
                            // background-color: #2d0008;
                            z-index: 2;
                        }
                    }
                }
            }
        }
        .price-depreciate {
            font: 16px "\5FAE\8F6F\96C5\9ED1";
            font-weight: 700;
            color: #fc5a5d;
            border-radius: 2px;
            background: url();
            padding-left: 12px;
        }
        .order {
            width: calc(100% + 12px);
            height: calc((100% - 44px) * (1 - 0.55));
            position: relative;
            right: 4px;
            > p {
                width: 100%;
                height: 30px;
                color: #1365a3;
                margin-top: 30px;
                padding-left: 24px;
                background: url(../../../static/imgs/computer.png) no-repeat 2px -2px;
            }
            .order1 {
                position: relative;
                width: 100%;
                height: calc((100% - 58px));
                margin-left: -16px;
                border: 26px solid transparent;
                border-image: url(../../../static/imgs/border.png) 30 30 round;
                p {
                    width: calc(100% + 15px);
                    height: 20px;
                    margin-left: -10px;
                    margin-top: -10px;
                    span {
                        line-height: 20px;
                        font-size: 12px;
                        float: left;
                    }
                    span:nth-child(1) {
                        width: 33%;
                        text-align: left;
                        white-space: nowrap;
                    }
                    span:nth-child(2) {
                        width: 33%;
                        text-align: right;
                        white-space: nowrap;
                    }
                    span:nth-child(3) {
                        width: 33%;
                        text-align: right;
                    }
                }
                ul {
                    width: calc(100% + 15px);
                    height: calc(100% - 11px);
                    margin-left: -10px;
                    overflow: hidden;
                    margin-bottom: 10px;
                    overflow-x: hidden;
                    overflow-y: auto;
                    li {
                        width: 100%;
                        height: 16px;
                        line-height: 16px;
                        span {
                            float: left;
                            line-height: 16px;
                            font-size: 12px;
                        }
                        span:nth-child(1) {
                            width: 33%;
                            text-align: left;
                        }
                        span:nth-child(2) {
                            width: 33%;
                            text-align: right;
                        }
                        span:nth-child(3) {
                            width: 33%;
                            text-align: right;
                        }
                    }
                }
            }
        }
    }
}
</style>
