import Vue from 'vue'
import Router from 'vue-router'

import cookie from "../../static/js/cookie";

const HelloWorld = () =>import ('@/components/HelloWorld')
const c2c = () =>import ('@/components/c2c/index') //c2c交易
const order = () => import('@/components/c2c/order') //c2c 订单
const coin = () =>import ('@/components/coin/index') //币币交易
const contract = () =>import ('@/components/contract/index') //合约交易

const newassets = () =>import('@/components/newassets/index')//资产页面
const chargeIn =() =>import('@/components/newassets/chargeIn')//币币账户中的充币页面
const moneyOut =() =>import('@/components/newassets/moneyOut')//币币账户中的提现页面
const chargeHistory =() =>import('@/components/newassets/chargeHistory')//币币账户中的历史记录
const c2cOrder =() =>import('@/components/newassets/c2cOrder')//c2c订单

// 登录
const login = () =>import ('@/components/login/login')
const register = () => import('@/components/login/register')
const forget = () => import('@/components/login/forget')

//个人中心
const user = () =>import ('@/components/user/user')
const invite = () =>import ('@/components/user/invite')
const realName = () =>import ('@/components/user/realName')
const seniorCertification = () =>import ('@/components/user/seniorCertification')
const walletAddress = () =>import ('@/components/user/walletAddress')

// 广告
const advert = () =>import ('@/components/advert/advert')
const releaseAdvert = () =>import ('@/components/advert/releaseAdvert')

//声明 关于我们
const about = () =>import ('@/components/statement/about')


Vue.use(Router)


  const routes=[
    {
      path: '/',
      name: 'c2c',
      component: c2c,
      meta: {  }
    },
    {
      path: '/c2c',
      name: 'c2c',
      component: c2c,
      meta: { }
    },
    {
      path: '/coin',
      name: 'coin',
      component: coin,
      meta: {
        hidenav:true
      }
    },
    {
      path: '/contract',
      name: 'contract',
      component: contract,
      meta: {
        hidenav:true
      }
    },
    {
      path: '/order/:id?',
      name: 'order',
      component: order,
      meta: {}
    },
    {
      path: '/newassets',
      name: 'newassets',
      component: newassets,
      meta: {
          auth: true,
      },
    },
    {
      path: '/chargeIn',
      name: 'chargeIn',
      component: chargeIn,
      meta: {
          auth: true,
      },
    },
    {
      path: '/moneyOut',
      name: 'moneyOut',
      component: moneyOut,
      meta: {
          auth: true,
      },
    },
    {
      path: '/chargeHistory',
      name: 'chargeHistory',
      component: chargeHistory,
      meta: {
          auth: true,
      },
    },
    {
      path: '/c2cOrder',
      name: 'c2cOrder',
      component: c2cOrder,
      meta: {
          auth: true,
      },
    },
    { //登录
      path: '/login',
      name: 'login',
      component: login,
      meta: {},
    },
    { // 注册
      path: '/register/:id?',
      name: 'register',
      component: register,
      meta: {},
    },
    { // 找回密码
      path: '/forget',
      name: 'forget',
      component: forget,
      meta: {},
    },
    { // 个人中心
      path: '/user',
      name: 'user',
      component: user,
      meta: {},
    },
    { // 邀请
      path: '/invite',
      name: 'invite',
      component: invite,
      meta: {},
    },
    { // 实名认证
      path: '/realName',
      name: 'realName',
      component: realName,
      meta: {},
    },
    { // 高级认证
      path: '/seniorCertification',
      name: 'seniorCertification',
      component: seniorCertification,
      meta: {},
    },
    { // 地址管理
      path: '/walletAddress',
      name: 'walletAddress',
      component: walletAddress,
      meta: {},
    },
    { // 广告列表
      path: '/advert',
      name: 'advert',
      component: advert,
      meta: {},
    },
    { // 发布列表
      path: '/releaseAdvert',
      name: 'releaseAdvert',
      component: releaseAdvert,
      meta: {},
    },
    { // 发布列表
      path: '/about/:id',
      name: 'about',
      component: about,
      meta: {},
    },
  ]


const router = new Router({
  routes
})
// router.beforeEach((to, from, next) => {
//   var userInfo = cookie.get('_auth') //router.app.$cookie.get('_auth') //JSON.parse(localStorage.getItem('userInfoStorage'));
//   //
//   //
//   if (to.meta.auth) { //判断需不需要登录验证
//     if (userInfo) { //有没有用户信息
//       next()
//     } else { //没有跳到登录页
//       next({
//         path: '/login',
//         query: {
//           redirect: to.fullPath
//         }
//       })
//     }
//   } else if (to.name === 'login') { //判断进入的地址是不是登录页
//     if (userInfo) { //有没有用户信息 有就返回首页 没有就进
//       next()
//     } else {
//       next()
//     }
//   } else {
//     //
//     next()
//   }
//   //
// })

export default router
