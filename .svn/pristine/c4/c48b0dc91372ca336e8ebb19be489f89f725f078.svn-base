<template>
    <div class="">
        <!-- 头 -->
        <div id="head">
            <p>
                <router-link to="/">
                    <img style="width:125px;" src="../../../static/imgs/logo.png">
                </router-link>
            </p>
            <ul>
                <li @click="$router.push(i.path)" v-for="(i,index) in tab" :key="index" :class="{'active':$route.path==i.path}">{{i.name}}</li>                
            </ul>
            <div id="user">
                <template v-if="userinfo">
                    <ul class="llt3 capital" style="display: block;">
                        <img alt="" class="money" src="../../../static/imgs/money-2.png">
                        <li class="tl1" id="li-1" style="display: block;" @click="$router.push('/newassets')">
                            资产
                            <!-- <span></span> -->
                        </li>
                        <!-- <div class="hidden-box hidden-loc-index" id="box_1">
                        <ul>
                            <li>
                                <a href="javascript:void(0);">资产管理</a>
                            </li>
                            <li>
                                <a href="#/address">地址管理</a>
                            </li>
                            <li>
                                <a href="#/in">充值</a>
                            </li>
                            <li>
                                <a href="#/out" style="border:none">提现</a>
                            </li>
                        </ul>
                    </div> -->
                    </ul>
                    <a class="sign-name" style="color: white; display: block;" @click="$router.push('/user')">{{user.nickName}}
                        <div class="sign-ul" @click.stop="logout">
                            <span>退出登录</span>
                        </div>
                    </a>

                    <p style=""></p>
                </template>
                <div class="head-sign" v-if="!userinfo">
                    <router-link to="/login">  登录</router-link>
                    <router-link to="/register">注册</router-link>
                </div>
            </div>

        </div>
        <p class="head-line"></p>
    </div>
</template>

<script type="">
export default {
    name: "tradeheader",
    data() {
        return {
            userinfo: this.$cookie.get("_auth"),
            user: {},
            tab:[
                {
                    name:'C2C交易',
                    path:'/c2c'
                },
                {
                    name:'币币交易',
                    path:'/coin'
                },
                {
                    name:'合约交易',
                    path:'/contract'
                }

            ],
        };
    },
    components: {},
    created() {
        this.getuserinfo();
        console.log();
        
    },
    methods: {
        // 获取用户信息
        getuserinfo() {
            if (!this.userinfo) {
                return;
            }
            this.$axios
                .post("api/user/query_active_user")
                .then(res => {
                    console.log(res);
                    this.user = res.data.data;
                })
                .catch(err => {});
        },
        logout() {
            this.$cookie.clear();
            this.userinfo=false
            // this.$route.push('/login')
        }
    }
};
</script>

<style scoped lang="less">
.active{
    color:#1987da !important;
}
.sign-name:hover .sign-ul {
    display: block;
}

.sign-ul {
    position: absolute;
    top: 37px;
    /* right: 68px; */
    z-index: 2;
    background: #fff;
    color: #000;
    padding: 5px 10px;
    border-radius: 4px;
    display: none;
    span {
        float: left;
        padding-top: 4px;
    }
}
.head-sign {
    float: left;
    font-size: 14px;
    padding-right: 68px;
    line-height: 20px;
    padding-top: 11px;
    > a {
        padding-left: 15px;
        color: #1daaec;
    }
}
// 头
#head {
    width: 100%;
    height: 45px;
    font-weight: bold;
    margin: 0 auto;
    background-color: #000;
    box-sizing: border-box;
    > p {
        padding-left: 16px;
        padding-right: 66px;
        float: left;
        font-size: 12px;
        width: 226px;
        height: 100%;
        color: white;
        line-height: 45px;
        a {
            cursor: pointer;
        }
    }
    > ul {
        width: 660px;
        height: 100%;
        float: left;
        li {
            float: left;
            line-height: 45px;
            color: white;
            margin-left: 64px;
            font-size: 14px;
            font-weight: 100;
            cursor: pointer;
        }
    }
    #user {
        width: 360px;
        height: 100%;
        float: right;
        position: relative;
        .capital {
            list-style-type: none;
            position: relative;
            .money {
                position: absolute;
                top: 0;
                left: 0;
                margin-top: 16px;
                margin-left: -20px;
            }
            > li {
                font-size: 14px;
                font-weight: 100;
                line-height: 45px;
                display: block;
                float: left;
                padding-right: 20px;
                color: #fff;
                margin-right: 1px;
                cursor: pointer;
            }
            span {
                display: inline-block;
                margin-left: 14px;
                width: 7px;
                height: 7px;
                background: url(../../../static/imgs/down.png) no-repeat;
            }
            .hover-down {
                transition-duration: 0.5s;
                transform: rotate(0deg);
            }
            .hover-up {
                transition-duration: 0.5s;
                transform: rotate(180deg);
            }
            .hidden-box {
                width: 100px;
                position: absolute;
                // display: none;
                top: 40px;
                left: 0;
                margin-left: -30px;
                background-color: rgb(0, 68, 120, 0.8);
                z-index: 100;
                li a {
                    width: 100%;
                    text-align: center;
                    border-bottom: 1px solid #13598f;
                    color: #fff;
                    padding-bottom: 10px;
                }
            }
        }
        > a {
            float: left;
            color: white;
            padding: 0 10px;
            margin-right: 46px;
            margin-top: 10px;
            outline: none;
            border: none;
            appearance: none;
            font-size: 14px;
            color: #1daaec;
            font-weight: 100;
        }
        > a:nth-child(2) {
            width: 156px;
            line-height: 26px;
            padding-left: 30px;
            background: url(../../../static/imgs/user.png) no-repeat;
            background-color: black;
            margin-left: 10px;
        }
        p {
            width: 30px;
            height: 20px;
            float: left;
            margin-right: 20px;
            margin-top: 10px;
            color: white;
            background: url(../../../static/imgs/CN.png) no-repeat 0 -2px;
        }
    }
}
.head-line {
    width: 100%;
    height: 1px;
    background: linear-gradient(
        to right,
        rgba(0, 0, 0, 0) 0%,
        rgba(0, 0, 0, 0) 10%,
        rgba(0, 0, 0, 0) 20%,
        #06212d 30%,
        #09374c 40%,
        #09374c 50%,
        #09374c 60%,
        #06212d 70%,
        #06212d 80%,
        rgba(0, 0, 0, 0) 90%,
        rgba(0, 0, 0, 0) 100%
    );
}
</style>
