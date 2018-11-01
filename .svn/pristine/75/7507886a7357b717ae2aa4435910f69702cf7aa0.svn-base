<template>
    <div id="Footer">
        <div class="footer">
            <div class="foot-top clearfix">
                <div class="foot-top-left"><img src="data:image/png;base64,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"></div>
                <div class="foot-top-in">
                    <ul>
                        <li>
                            <a href="https://www.gmqcoin.com/#/content?id=19" target="_blank">关于我们</a>
                        </li>
                        <li>
                            <a href="https://support.gmqcoin.com/#/notice?parentId=8" target="_blank">公告中心</a>
                        </li>
                        <li>
                            <a href="https://www.gmqcoin.com/#/content?id=14" target="_blank">费率说明</a>
                        </li>
                        <li>
                            <a href="https://www.gmqcoin.com/#/content?id=4" target="_blank">服务条款</a>
                        </li>
                    </ul>
                </div>
                <div class="foot-top-in">
                    <ul>
                        <li>
                            <a href="https://support.gmqcoin.com/#/notice?parentId=1" target="_blank">帮助中心</a>
                        </li>
                        <li>
                            <a href="https://support.gmqcoin.com/#/notice?parentId=12" target="_blank">新闻中心</a>
                        </li>
                        <li>
                            <a href="https://www.gmqcoin.com/#/content?id=13" target="_blank">法律声明</a>
                        </li>
                        <li>
                            <a href="https://www.gmqcoin.com/#/content?id=15" target="_blank">隐私声明</a>
                        </li>
                    </ul>
                </div>
                <div class="foot-top-right">
                    <span>联系我们</span>
                    <div class="foot-tel clearfix">
                        <span>中文地址:</span>
                        <em>菲律宾马尼拉大都会BGC商务中心</em>
                    </div>
                    <div class="foot-tel clearfix">
                        <span>英文地址:</span>
                        <em>Bonifacio,Metro Manila,Philippines.</em>
                    </div>
                    <div class="foot-tel">
                        <i class="iconfont icon-weibiaoti-1"></i>
                        <span>邮箱:</span>
                        <em>support@gmquickpaycoin.com</em>
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
                <p>Copyright (c) 2018 GMQ Group All Rights Reserved</p>
            </div>
        </div>
    </div>
</template>

<script type="">
export default {
    name: "",
    data() {
        return {};
    },
    components: {}
};
</script>

<style scoped lang="less">
#Footer{
    background: #201a40;
    .footer{
        width: 1200px;
        margin: 0 auto;
        padding-top: 48px;
        .foot-top{
            border-bottom: 1px solid #4e5768;
            padding-bottom: 40px;
            .foot-top-left{
                padding: 70px 50px 0 0;
                float: left;
                > img{
                    width: 224px;
                }
            }
            .foot-top-in{
                float: left;
                padding: 0 78px;
                font-size: 15px;
                color: #a4a6ab;
                line-height: 38px;
                li > a{
                    color: #a4a6ab;
                }
            }
            .foot-top-right{
                float: left;
                padding-left: 56px;
                font-size: 15px;
                color: #616978;
                line-height: 38px;
                .foot-tel > span{
                    float: left;
                    padding-right: 5px;
                }
                .foot-tel > em{
                    float: left;
                }
                .foot-tel > i{
                    font-size: 20px;
                    float: left;
                    padding-right: 5px;
                }
            }
        }
        .foot-down{
            text-align: center;
            padding-bottom: 58px;
            .friendship{
                color: #616978;
                padding-top: 26px;
                font-size: 14px;
                > a{
                    color: #616978;
                    padding: 0 8px;
                    display: inline-block;
                }
            }
            p{
                padding-top: 30px;
                font-size: 15px;
                color: #616978;
            }
        }
    }
}
</style>
