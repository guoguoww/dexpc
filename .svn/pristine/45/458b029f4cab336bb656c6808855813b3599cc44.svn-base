<template>
    <div id="Footer">
        <div class="footer">
            <div class="foot-top clearfix">
                <div class="foot-top-left"><img src="../../../static/imgs/logo.png"></div>
                <div class="foot-top-in">
                    <ul>
                        <li>
                            <router-link to='/about/1' target="_blank">关于我们</router-link>
                        </li>
                        <li>
                            <router-link to='/about/2' target="_blank">服务条款</router-link>
                        </li>
                    </ul>
                </div>
                <div class="foot-top-in">
                    <ul>
                        <li>
                            <router-link to='/about/3' target="_blank">法律声明</router-link>
                        </li>
                        <li>
                            <router-link to='/about/4' target="_blank">隐私声明</router-link>
                        </li>
                    </ul>
                </div>
                <div class="foot-top-right">
                    <span>联系我们</span>
                    <div class="foot-tel clearfix">
                        <span>中文地址:</span>
                        <em>新加坡共和国沙滩路152号14-02号</em>
                    </div>
                    <div class="foot-tel clearfix">
                        <span>英文地址:</span>
                        <em>152BEACH ROAD#14-02 GATEWAY EAST SINGAPORE 189721 </em>
                    </div>
                    <div class="foot-tel">
                        <!-- <i class="iconfont icon-weibiaoti-1"></i> -->
                        <span>客服QQ:</span>
                        <em>838314752</em>
                    </div>
                </div>
            </div>
            <div class="foot-down">
                <div class="friendship">
                    <span>友情链接:</span>
                    <a href="#">金色财经</a>
                    <a href="#">和讯网</a>
                    <a href="#">区块网</a>
                    <a href="#">币视界</a>
                    <a href="#">非小号</a>
                    <a href="#">比特币中国</a>
                    <a href="#">巴比特</a>
                </div>
                <p>Copyright (c) 2018 DSDEX Group All Rights Reserved</p>
            </div>
            <div class="qrcode">
                <div class="box"><div id="ios"><img src="../../../static/imgs/ios.png" alt=""></div><p>Ios下载地址</p></div>
                <div class="box"><div id="an"><img src="../../../static/imgs/android.png" alt=""></div><p>Android下载地址</p></div>
            </div>
        </div>
    </div>
</template>

<script type="">
import QRCode from "qrcodejs2";

export default {
    name: "footerc",
    data() {
        return {};
    },
    components: { QRCode },
    mounted () {
        // this.ios('这是一个什么内容都没有的二维码')
        // this.an('这是一个什么内容都没有的二维码')
    },
    methods: {
        ios(text) {
            let qrcode = new QRCode("ios", {
                width: 125, // 设置宽度
                height: 125, // 设置高度
                text
            });
        },
        an(text) {
            let qrcode = new QRCode("an", {
                width: 125, // 设置宽度
                height: 125, // 设置高度
                text
            });
        },
    }
};
</script>

<style scoped lang="less">
.box{
    >div{
        width:125px;
        height: 125px;
        img{
            height: 100%;
            width: 100%;
        }
    }
}
.qrcode{
    display: flex;
    justify-content: space-around;
    width: 27%;
    position: absolute;
    top: 57px;
    right: -27%;
    p{
        margin-top: 10px;
        color: #ccc;
        text-align: center;
    }
}
#Footer {
    background: #201a40;
    .footer {
        position: relative;
        width: 1200px;
        margin: 0 auto;
        padding-top: 48px;
        .foot-top {
            border-bottom: 1px solid #4e5768;
            padding-bottom: 40px;
            .foot-top-left {
                padding: 70px 50px 0 0;
                float: left;
                > img {
                    // width: 224px;
                }
            }
            .foot-top-in {
                float: left;
                padding: 0 78px;
                font-size: 15px;
                color: #a4a6ab;
                line-height: 38px;
                li > a {
                    color: #a4a6ab;
                }
            }
            .foot-top-right {
                float: left;
                padding-left: 56px;
                font-size: 15px;
                color: #616978;
                line-height: 38px;
                .foot-tel > span {
                    float: left;
                    padding-right: 5px;
                }
                .foot-tel > em {
                    float: left;
                }
                .foot-tel > i {
                    font-size: 20px;
                    float: left;
                    padding-right: 5px;
                }
            }
        }
        .foot-down {
            text-align: center;
            padding-bottom: 58px;
            .friendship {
                color: #616978;
                padding-top: 26px;
                font-size: 14px;
                > a {
                    color: #616978;
                    padding: 0 8px;
                    display: inline-block;
                }
            }
            p {
                padding-top: 30px;
                font-size: 15px;
                color: #616978;
            }
        }
    }
}
</style>
