<template>
    <div>
      <!--&lt;!&ndash;分享&ndash;&gt;-->
      <!--<div class="share-link">-->
        <!--<router-link to="/invite"></router-link>-->
      <!--</div>-->
      <!--<div class="myAccount">-->
        <!--<div class="security">-->
          <!--<p>-->
            <!--安全建议：检查访问网址、开启实名验证、不要给声称为GMQ工作人员转账和透露密码信息等。-->
          <!--</p>-->
        <!--</div>-->
        <!--<div class="basicInfo clearfix">-->
          <!--<div class="headImg fl">-->
            <!--<img src="../../../static/imgs/img-user.png"/>-->
          <!--</div>-->
          <!--<div class="info fl">-->
            <!--<p class="nickName">-->
              <!--<span>{{userData.nickName}}</span>-->
              <!--<button v-if="!userData.nickNameUpdateTime" @click="sheupNickname()">设置</button>-->
            <!--</p>-->
            <!--<p class="loginTime">邀请码: {{userData.inviteCode}}</p>-->
          <!--</div>-->
          <!--<div class="registeredInfo fr">-->
            <!--<p class="fl">总成单次数: {{userData.tradeCount? 'userData.tradeCount': '0'}}</p>-->
            <!--<p class="fl">本账号于 {{userData.registerTime}}  注册</p>-->
          <!--</div>-->
        <!--</div>-->
        <!--<div class="securityCenter clearfix">-->
          <!--&lt;!&ndash;修改密码&ndash;&gt;-->
          <!--<div class="fl securityCenter_box">-->
            <!--<div class="modifyPwd panel">-->
              <!--<div class="panel-title">登录密码</div>-->
              <!--<div class="panel-body">-->
                <!--<div class="panel-item">-->
                  <!--<Icon class="imgicon" type="ios-unlock" />-->
                  <!--<h4 class="">登录密码</h4>-->
                  <!--<p class="">登录时使用</p>-->
                  <!--<button class="btn" @click="sheupPwd()">修改</button>-->
                <!--</div>-->
              <!--</div>-->
            <!--</div>-->
          <!--</div>-->
          <!--&lt;!&ndash;实名认证&ndash;&gt;-->
          <!--<div class="fr securityCenter_box">-->
            <!--<div class="authentication panel">-->
              <!--<div class="panel-title">实名认证</div>-->
              <!--<div class="panel-body">-->
                <!--<div class="panel-item">-->
                  <!--<Icon class="imgicon" type="ios-card-outline" />-->
                  <!--<h4 class="">身份认证</h4>-->
                  <!--<p class="">完成实名认证可进行小额交易</p>-->
                  <!--<button class="btn" >认证</button>-->
                <!--</div>-->
                <!--<div class="panel-item">-->
                  <!--<Icon class="imgicon" type="ios-card-outline" />-->
                  <!--<h4 class="">高级认证</h4>-->
                  <!--<p class="">完成高级认证可进行比较大提现额度</p>-->
                  <!--<button class="btn" >认证</button>-->
                <!--</div>-->
              <!--</div>-->
            <!--</div>-->
          <!--</div>-->
        <!--</div>-->
        <!--<div class="payType">-->
          <!--<div class="payType-title clearfix">-->
            <!--<h3 class="fl">支付方式</h3>-->
            <!--<router-link class="fr" to="">添加支付方式</router-link>-->
          <!--</div>-->
          <!--<ul class="payType-list">-->
            <!--<li class="payType-item"></li>-->
          <!--</ul>-->
          <!--<p class="notAvailable">暂无支付方式…</p>-->
        <!--</div>-->
      <!--</div>-->
      <div id="personal">
        <div class="personal">
          <!--头部-->
          <div class="personal-head">
            <div class="per-image">{{userData.nickName? userData.nickName[0]: '空'}}</div>
            <em>个人中心</em>
            <span>昵称：{{userData.nickName? userData.nickName : '暂未设置'}}</span>
            <button v-if="!userData.nickNameUpdateTime" @click="sheupNickname()">设置</button>
            <span>总成单：{{userData.tradeCount? userData.tradeCount: '0'}}次 </span>
            <span>本账号于 {{userData.registerTime}} 注册 , {{userData.tradeCount? '': '尚未交易'}}</span>
          </div>
          <!--账户安全-->
          <div class="personal-list">
            <div class="personal-list-head clearfix">
              <span >账户与安全</span>
            </div>
            <div class="personal-list-tab">
              <table border="0" cellspacing="0" cellpadding="0">
                <tr>
                  <td>
                    <Icon type="md-person-add" />
                    <span>邀请推广</span>
                  </td>
                  <td>
                    <span>{{userData.inviteCode}}</span>
                  </td>
                  <td>
                    <router-link to="/invite">邀请</router-link>
                  </td>
                </tr>
                <tr>
                  <td>
                    <Icon class="imgicon" type="ios-lock" />
                    <span>登录密码</span>
                  </td>
                  <td>
                    <span>******</span>
                  </td>
                  <td>
                    <a href="javascript:void (0);" @click="sheupPwd()">修改</a>
                  </td>
                </tr>
                <tr>
                  <td>
                    <Icon class="imgicon" type="md-navigate" />
                    <span>地址管理</span>
                  </td>
                  <td>
                    <span>充值币地址</span>
                  </td>
                  <td>
                    <router-link to="/walletAddress">管理</router-link>
                  </td>
                </tr>
              </table>
            </div>
          </div>
          <!--身份认证-->
          <div class="personal-list">
            <div class="personal-list-head clearfix">
              <span >身份认证</span>
              <i>请如实填写您的身份信息，一经认证不可修改</i>
            </div>
            <div class="personal-list-tab">
              <table border="0" cellspacing="0" cellpadding="0">
                <tr>
                  <td>
                    <Icon class="imgicon" type="ios-card-outline" />
                    <span>实名认证</span>
                  </td>
                  <td>
                    <span>{{userData.authStatusGeneral === '2'? userData.chnName + '， ' +userData.idCard : authStatusGeneralType}}</span>
                  </td>
                  <td>
                    <router-link v-if="userData.authStatusGeneral === '0' || userData.authStatusGeneral === '3' " to="/realName">认证</router-link>
                    <router-link v-if="userData.authStatusGeneral === '1' " to="">认证中</router-link>
                  </td>
                </tr>
                <!-- <tr>
                  <td>
                    <Icon class="imgicon" type="ios-card-outline" />
                    <span>高级认证</span></td>
                  <td>
                    <span>{{userData.authStatusGeneral === '2' ? authStatusAdvancedType : '请先进行实名认证'}}</span>
                  </td>
                  <td>
                    <router-link v-if="userData.authStatusGeneral === '2' && (userData.authStatusAdvanced === '0' || userData.authStatusAdvanced === '3') " to="/seniorCertification">认证</router-link>
                    <router-link v-if="userData.authStatusGeneral === '2' && userData.authStatusAdvanced === '1' " to="">认证中</router-link>
                  </td>
                </tr> -->
              </table>
            </div>
          </div>
          <!--支付方式-->
          <div class="personal-list">
            <div class="personal-list-head clearfix">
              <span>支付方式</span>
              <i>请务必使用您本人的实名账户，被激活的支付方式将在交易时向买方展示</i>
            </div>
            <div class="personal-list-tab">
              <table border="0" cellspacing="0" cellpadding="0">
                <tr v-for="(item,i) in payList.data" :key="i">
                  <td>
                    <!-- <img :src="item.type === '0'? alisrc: (item.type === '1'? wxsrc : banksrc)"/> -->
                    <img src="../../../static/imgs/ali.png" v-if="item.type == 0">
                    <img src="../../../static/imgs/wx.png" v-else-if="item.type == 1">
                    <img src="../../../static/imgs/bank.png" v-else>
                    <span>{{item.type === '0'? '支付宝': (item.type === '1'? '微信' : '银行卡')}}</span>
                  </td>
                  <td>
                    <span>{{item.account}}</span>
                  </td>
                  <td>
                    <a href="javascript: void (0);" @click="getPayDetails(item.id)">修改</a>
                    <a href="javascript: void (0);" @click="removePayType(item.id)">删除</a>
                  </td>
                </tr>
              </table>
              <div class="personal-add" @click="payTypeAdd()"><span>点击添加支付方式</span></div>
              <!--分页器-->
              <div class="page" v-if="payList.total > pages.rows">
                <Page :total="payList.total" :page-size="pages.rows" size="small" show-total show-elevator @on-change="pageChange" />
              </div>
            </div>
          </div>
        </div>
      </div>

      <!--朦版 修改昵称or密码-->
      <div class="background-000" v-if="modalShow"></div>
      <div class="modify" v-if="modalShow">
        <!--头部-->
        <div class="modify_title">
          <h3>{{modalCon? '昵称': '密码'}}修改</h3>
          <Icon class="close" type="ios-close" @click="cancel()"/>
        </div>
        <!--昵称修改-->
        <div class="modify_input_pwd" v-if="modalCon">
          <input type="text" placeholder="建议不使用真实姓名,设置后不可更改" v-model="nickName" @keyup.enter="submit(modalCon)" >
        </div>
        <!--密码修改-->
        <div class="modify_input_pwd" v-if="modalCon === false">
          <input type="password" placeholder="旧密码" v-model="pwd.password" @keyup.enter="submit(modalCon)">
          <input type="password" placeholder="新密码" v-model="pwd.newPassword" @keyup.enter="submit(modalCon)">
          <input type="password" placeholder="请再次输入新密码" v-model="confirmPwd" @keyup.enter="submit(modalCon)">
        </div>
        <!--按钮-->
        <div class="confirm-close clearfix">
          <a href="javascript:void (0)" class="closeBtn fl" @click="cancel()">取消</a>
          <a href="javascript:void (0)" class="fl" @click="submit(modalCon)">确认</a>
        </div>
      </div>

      <!--添加支付方式-->
      <div id="payPop" v-if="payTypeAddShow">
        <div class="pay-pop">
          <div class="pay-pop-cont">
            <div class="pay-head">
              <span>{{openpayType === 'add'? '添加': '修改'}}支付方式</span>
              <Icon  @click="submitCancel()" class="close" type="ios-close" />
            </div>
            <ul class="pay-list">
              <li v-if="!payData.id">
                <span>支付方式</span>
                <select v-model="typeDefault" @change="typeChage()" >
                  <option v-for="item in typeList"  :value="item.value">{{item.name}}</option>
                </select>
              </li>
              <li v-if="payData.id">
                <span>支付方式</span>
                <input class="readonly" type="text" name="name" value="" v-model="typeShift.typeName" readonly>
              </li>
              <li>
                <span>姓名</span>
                <input class="readonly" type="text" name="name" value="" v-model="userData.chnName" readonly>
              </li>
              <li v-if="typeShift.openBank">
                <span>开户银行</span>
                <input type="text" value="" placeholder="请输入开户银行" v-model="payData.bankName">
              </li>
              <li v-if="typeShift.openBranch">
                <span>开户支行</span>
                <input type="text" value="" placeholder="请输入详细具体支行" v-model="payData.openBankName">
              </li>
              <li>
                <span>{{typeShift.typeName}}账号</span>
                <input type="text" value="" :placeholder="'请输入'+ typeShift.typeName +'账号'" v-model="payData.account">
              </li>
              <li v-if="typeShift.twoPic">
                <span>二维码</span>
                <div class="acc-prog">
                  <div id="uploadw" v-show="!payData.image" @change="changImg('qrcodePic', $event)">
                    <label>
                      <input type="file" id="file" name="file" accept="image/*" >
                      <div class="acc-cont-up">
                        <p>请上传您的收款二维码图片(*.jpg / *.png / *.jpeg)</p>
                      </div>
                    </label>
                  </div>
                  <div class="prog-img" v-show="payData.image">
                    <i @click="payData.image = ''">删除</i>
                    <img id="qrcodePic" :src="!!payData.image ? payData.image : ''"/>
                  </div>
                </div>
              </li>
              <li>
                <span>备注</span>
                <input type="text" value="" placeholder="备注" v-model="payData.remark">
              </li>
            </ul>
            <div class="pay-but">
              <button @click="submitCancel()">取消</button>
              <button class="confirm" @click="submitData(openpayType)">保存</button>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
  import $ from "jquery"
  export default {
    name: "user",
    data() {
      return {
        userData: {},
        nickName: '', //昵称
        pwd: {
          password: '', //旧密码
          newPassword: '', //新密码
        },
        confirmPwd: '', //再次确认密码
        modalShow: false, //模态框
        modalCon: null, // 模态框设置内容 true为昵称 false为密码
        payList: { //支付列表
          data: [],
          total: 0,
        },
        pages: { //分页
          start: 0,
          rows: 5,
        },
        payTypeAddShow: false, //支付方式添加框
        openpayType: 'add', //打开支付列表模态是修改还是添加
        typeDefault: '0',  //支付类型初始
        typeList: [ //支付类型选择
          {
            name: '支付宝',
            value: '0'
          },
          {
            name: '微信',
            value: '1'
          },
          {
            name: '银行卡',
            value: '2'
          },
        ],
        typeShift: { //支付方式转换 切换不同内容
          typeName: '支付宝', //类型账号
          twoPic: true, //二维码显示
          openBank: false, //开户银行选择显示
          openBranch: false, //开户支行填写显示
        },
        payData: {
          account: '', //账号
          bankName: '', //银行名称
          image: '', //二维码图片内容
          openBankName: '', //开户行支行地址
          remark: '', //备注
          type: '0', //收付款方式 0 支付宝 1 微信 2 银行卡
        },
          // alisrc:'../../../static/imgs/ali.png',
          // wxsrc:'../../../static/imgs/wx.png',
          // banksrc:'../../../static/imgs/bank.png'
      }
    },
    computed: {
      authStatusGeneralType (){ //普通认证状态
        switch(this.userData.authStatusGeneral){
          case '0':
            return '未认证'
          case '1':
            return '认证中'
          case '3':
            return '认证失败'
        }
      },
      authStatusAdvancedType (){ //普通认证状态
        switch(this.userData.authStatusAdvanced){
          case '0':
            return '未认证'
          case '1':
            return '认证中'
          case '2':
            return '已认证'
          case '3':
            return '认证失败'
        }
      },
    },
    created() {
      this.getUserData();
    },
    methods: {
      getUserData() {  //获取用户信息
        this.$axios.post("/api/user/query_active_user")
          .then(res => {
            if(res.data.errorCode === 0) {
              this.userData = res.data.data;
              this.getPayList();
            }
          })
      },
      sheupNickname() { //设置昵称
        this.modalShow = true;
        this.modalCon = true;
      },
      sheupPwd() { //设置密码
        this.modalShow = true;
        this.modalCon = false;
      },
      cancel() { //模态取消
        this.modalShow = false;
        this.modalCon = false;
      },
      submit(modalCon) { //昵称or密码提交
        if(modalCon === true) { //提交新昵称
          if(this.nickName === '') {
            this.$Message.warning('请输入昵称');
            return
          }
          this.$axios({
            url: '/api/user/modify/nick_name',
            method: 'post',
            data: {
              nickName: this.nickName,
            }
          }).then(res => {
            if(res.data.errorCode === 0) {
              this.$Message.success('设置成功');
              this.modalShow = false;
              this.getUserData()
            }
          })
        }
        if(modalCon === false) { //提交新密码
          if(this.pwd.password === '') {
            this.$Message.warning('请输入旧密码');
            return;
          }
          if(this.pwd.newPassword === '') {
            this.$Message.warning('请输入新密码');
            return;
          }
          if(this.confirmPwd !== this.pwd.newPassword) {
            this.$Message.warning('两次输入密码不同');
            return;
          }
          this.$axios({
            url: "/api/user/updatepassword",
            method: "post",
            data: this.pwd,
          }).then(res => {
            if(res.data.errorCode === 0) {
              this.$Message.success('设置成功');
              this.$cookie.clear(); //清除cookie
              this.$router.push('/login')//跳转登录页
            }
          })
            .catch(err => {});
        }
      },

      getPayList() { //获取支付列表
        this.$axios({
          url: "/api/user/paymentreceived",
          method: "post",
          params: this.pages,
        }).then(res => {
          if(res.data.errorCode === 0) {
            this.payList.data = res.data.page.data
            this.payList.total = res.data.page.total
          }
        })
      },
      typeChage() { //支付方式转换 切换不同内容
        if(this.typeDefault === '0') {
          this.typeShift.typeName = '支付宝';
          this.typeShift.twoPic = true;
          this.typeShift.openBank = false;
          this.typeShift.openBranch = false;
        }
        if(this.typeDefault === '1') {
          this.typeShift.typeName = '微信';
          this.typeShift.twoPic = true;
          this.typeShift.openBank = false;
          this.typeShift.openBranch = false;
        }
        if(this.typeDefault === '2') {
          this.typeShift.typeName = '银行卡';
          this.typeShift.twoPic = false;
          this.typeShift.openBank = true;
          this.typeShift.openBranch = true;
        }
        this.payData.account = ''; //账号初始
        this.payData.bankName = ''; //银行初始
        this.payData.image = ''; //二维码初始
        this.payData.openBankName = ''; //支行初始
        this.payData.remark = ''; //备注初始
        this.payData.type = this.typeDefault; //类型赋值
      },
      changImg(id, e) { //上传图片回显 //企业/组织营业执照 证件正反面
        var that = this;
        var id = id;
        for (var i = 0; i < e.target.files.length; i++) {
          var file = e.target.files.item(i);
          if (!(/^image\/.*$/i.test(file.type))) {
            continue; //不是图片 就跳出这一次循环
          }
          if(e.target.files[i].size > 5242880) { //大小不能超过5M
            this.$Message.warning('海报大小不能超过5M');
            continue
          }
          //实例化FileReader API
          var freader = new FileReader();
          freader.readAsDataURL(file);
          freader.onload = function(e) {
            document.getElementById(id).setAttribute('src', e.target.result);
            that.payData.image = e.target.result || '';
            document.getElementById("file").value = ""
          }
        }
      },
      submitData(type) { //提交支付方式
        if(this.payData.account === '') {
          this.$Message.warning('账号不能为空');
          return;
        }
        if((this.payData.type === '0' || this.payData.type === '1') && this.payData.image === '') {
          this.$Message.warning('请上传二维码');
          return;
        }
        if(this.payData.type === '2' && this.payData.bankName === '') {
          this.$Message.warning('银行名称不能为空');
          return;
        }
        if(type === 'add') {
          this.$axios({
            url: "/api/user/addpaymentreceived",
            method: "post",
            headers: {
              "Content-Type": "application/json;charset=utf-8"
            },
            data: this.payData
          }).then(res => {
            if(res.data.errorCode === 0) {
              this.$Message.success('添加成功')
              this.payTypeAddShow = false;
              this.openpayType = 'add'
              this.getPayList();

              this.typeDefault = '0';
              this.typeShift.typeName = '支付宝';
              this.typeShift.twoPic = true;
              this.typeShift.openBank = false;
              this.typeShift.openBranch = false;

              this.payData.account = ''; //账号初始
              this.payData.bankName = ''; //银行初始
              this.payData.image = ''; //二维码初始
              this.payData.openBankName = ''; //支行初始
              this.payData.remark = ''; //备注初始
              this.payData.type = '0'; //类型赋值
              this.payData.id = null;
            }
          })
            .catch(err => {});
        }
        if(type === 'modify') {
          this.$axios({
            url: "/api/user/modify/payment",
            method: "post",
            data: {
              id: this.payData.id,
              account:  this.payData.account,
              image: this.payData.image,
              bankName: this.payData.bankName,
              openBankName: this.payData.openBankName,
              remark: this.payData.remark
            },
          }).then(res => {
            if(res.data.errorCode === 0) {
              this.$Message.success('修改成功')
              this.payTypeAddShow = false;
              this.openpayType = 'add';
              this.getPayList();

              this.typeDefault = '0';
              this.typeShift.typeName = '支付宝';
              this.typeShift.twoPic = true;
              this.typeShift.openBank = false;
              this.typeShift.openBranch = false;

              this.payData.account = ''; //账号初始
              this.payData.bankName = ''; //银行初始
              this.payData.image = ''; //二维码初始
              this.payData.openBankName = ''; //支行初始
              this.payData.remark = ''; //备注初始
              this.payData.type = '0'; //类型赋值
              this.payData.id = null;
            }
          })
        }

      },
      submitCancel() { //取下上传
        this.payTypeAddShow = false;
        this.openpayType = 'add';
        this.getPayList();

        this.typeDefault = '0';
        this.typeShift.typeName = '支付宝';
        this.typeShift.twoPic = true;
        this.typeShift.openBank = false;
        this.typeShift.openBranch = false;

        this.payData.account = ''; //账号初始
        this.payData.bankName = ''; //银行初始
        this.payData.image = ''; //二维码初始
        this.payData.openBankName = ''; //支行初始
        this.payData.remark = ''; //备注初始
        this.payData.type = '0'; //类型赋值
        this.payData.id = null;
      },
      payTypeAdd() { //添加支付方式
        if(this.userData.authStatusGeneral === '0' || this.userData.authStatusGeneral === '3') {
          this.$Message.error('请先进行普通身份认证')
        }else if(this.userData.authStatusGeneral === '1') {
          this.$Message.warning('请等待申请通过')
        }else {
          this.payTypeAddShow = true
        }
      },
      removePayType(id) { //删除支付方式
        if(confirm("确认删除？")) {
          this.$axios({
            url: '/api/user/delPaymentReceived',
            method: "get",
            params: {
              id: id
            }
          }).then(res => {
            if(res.data.errorCode === 0) {
              this.$Message.success('删除成功');
              this.getPayList();
            }
          })
        }

      },
      getPayDetails(id) { //获取支付方式详情
        this.payTypeAddShow = true;
        this.openpayType = 'modify'
        this.$axios({
          url: "/api/user/findOnePaymentReceived",
          method: "get",
          params: {
            id: id
          },
        }).then(res => {
          if(res.data.errorCode === 0) {
            $.extend(this.payData, res.data.data)
            this.typeDefault = this.payData.type;
            if(this.typeDefault === '0') {
              this.typeShift.typeName = '支付宝';
              this.typeShift.twoPic = true;
              this.typeShift.openBank = false;
              this.typeShift.openBranch = false;
            }
            if(this.typeDefault === '1') {
              this.typeShift.typeName = '微信';
              this.typeShift.twoPic = true;
              this.typeShift.openBank = false;
              this.typeShift.openBranch = false;
            }
            if(this.typeDefault === '2') {
              this.typeShift.typeName = '银行卡';
              this.typeShift.twoPic = false;
              this.typeShift.openBank = true;
              this.typeShift.openBranch = true;
            }
            if(this.payData.image) {
              this.$axios.get('/api/image/'+ this.payData.image)
                .then(res => {
                  this.payData.image = res.data
                })
            }
          }
        })
      },

      pageChange(page) { //支付列表分页
        this.pages.start = this.pages.rows * (page-1);
        this.getPayList()
      },

    },
  }
</script>

<style scoped>
  #personal {
    width: 1200px;
    margin: 0 auto;
  }
  .personal {
    padding: 30px 0;
  }
  /*头部*/
  .personal-head {
    line-height: 48px;
    margin: 24px 0;
    background: #fff;
    padding: 0 60px;
    font-size: 14px;
  }
  .per-image {
    width: 34px;
    height: 34px;
    background: #4e96e1;
    color: #fff;
    font-weight: bolder;
    display: inline-block;
    text-align: center;
    font-size: 16px;
    border-radius: 50%;
    line-height: 34px;
  }
  .personal-head>em {
    padding: 0 10px;
  }
  .personal-head>span {
    padding-left: 60px;
  }
  /*昵称设置按钮*/
  .personal-head>button {
    font-size: 14px;
    height: 36px;
    line-height: 36px;
    outline: 0;
    border: 1px solid #ddd;
    background: #fff;
    margin: 8px 10px;
    padding: 0 10px;
    vertical-align: middle;
    cursor: pointer;
  }
  .personal-list {
    margin-bottom: 20px;
  }
  .personal-list-head {
    font-size: 16px;
    line-height: 50px;
    background: #e9eef8;
  }
  .personal-list-head>span {
    padding: 0 40px 0 180px;
    float: left;
  }
  .personal-list-head>i {
    font-size: 14px;
  }
  .personal-list-tab {
    padding: 20px 100px;
    background: #fff;
  }
  .personal-list-tab table {
    width: 100%;
  }
  .personal-list-tab td {
    padding: 20px 0;
    font-size: 15px;
    color: #404040;
    border-bottom: 1px solid #ededed;
    width: 27%;
  }
  .personal-list-tab td:first-child {
    padding-left: 78px;
  }
  .personal-list-tab td:nth-child(2) {
    width: 46%;
    text-align: center;
  }
  .personal-list-tab td:last-child {
    text-align: right;
    padding-right: 78px;
  }
  .personal-list-tab td>i {
    font-size: 20px;
    color: #7d7d7d;
    float: left;
    line-height: 15px;
    padding-right: 5px;
  }
  .personal-list-tab td>a {
    color: #3095ff;
    padding-left: 10px;
  }
  .personal-list-tab td>img {
    float: left;
    width: 26px;
    padding-right: 5px;
    margin-top: -3px;
  }
  .personal-add {
    text-align: center;
    padding: 20px 0;
    color: #007dff;
    font-size: 15px;
    cursor: pointer;
  }
  /*分页*/
  .page {
    text-align: center;
    padding: 10px 0;
    border-top: 1px solid #ededed;
  }



  /*!*分享*!*/
  /*.share-link {*/
    /*width: 100%;*/
    /*height: 90px;*/
    /*min-width: 1200px;*/
    /*background: url("../../../static/imgs/invite_rk.jpg") no-repeat center center;*/
  /*}*/
  /*.share-link a {*/
    /*display: inline-block;*/
    /*width: 100%;*/
    /*height: 100%;*/
  /*}*/
  /*.myAccount {*/
    /*width: 1200px;*/
    /*margin: 0 auto;*/
    /*padding: 0;*/
  /*}*/
  /*!*安全提醒*!*/
  /*.security {*/
    /*margin: 15px auto 0;*/
    /*color: #666;*/
    /*font-size: 12px;*/
    /*border: 1px solid #DDD;*/
    /*height: 40px;*/
    /*line-height: 40px;*/
    /*padding: 0 0 0 18px;*/
    /*position: relative;*/
    /*cursor: pointer;*/
  /*}*/
  /*!*基本信息*!*/
  /*.basicInfo {*/
    /*padding: 30px 0;*/
    /*border: 1px solid #ddd;*/
    /*position: relative;*/
    /*line-height: 100%;*/
    /*margin: 15px 0;*/
  /*}*/
  /*.basicInfo .headImg {*/
    /*width: 184px;*/
    /*text-align: center;*/
  /*}*/
  /*.basicInfo .headImg img {*/
    /*margin-bottom: 12px;*/
    /*border-radius: 50%;*/
  /*}*/
  /*.info .nickName {*/
    /*margin: 10px 0;*/
  /*}*/
  /*.info .nickName span {*/
    /*display: inline-block;*/
    /*height: 32px;*/
    /*line-height: 32px;*/
    /*vertical-align: middle;*/
    /*font-size: 18px;*/
    /*color: #999;*/
    /*font-weight: bold;*/
  /*}*/
  /*.info .nickName button {*/
    /*font-size: 14px;*/
    /*outline: 0;*/
    /*border: 1px solid #ddd;*/
    /*background: #fff;*/
    /*padding: 8px 10px;*/
    /*margin-left: 10px;*/
    /*vertical-align: middle;*/
    /*cursor: pointer;*/
  /*}*/
  /*.info .loginTime {*/
    /*font-size: 12px;*/
    /*color: #999;*/
  /*}*/
  /*.registeredInfo {*/
    /*height: 66px;*/
    /*line-height: 66px;*/
  /*}*/
  /*.registeredInfo p {*/
    /*margin: 0 50px;*/
  /*}*/
  /*!*安全中心*!*/
  /*.securityCenter_box {*/
    /*width: 586px;*/
  /*}*/
  /*.panel {*/
    /*width: 100%;*/
    /*border: 1px solid #ddd;*/
    /*margin-bottom: 15px;*/
  /*}*/
  /*.panel .panel-title {*/
    /*font-size: 14px;*/
    /*color: #666;*/
    /*padding: 0 15px;*/
    /*height: 36px;*/
    /*line-height: 36px;*/
    /*border-bottom: 1px solid #ddd;*/
  /*}*/
  /*.panel .panel-body {*/
    /*padding: 30px 15px;*/
  /*}*/
  /*.panel .panel-body .panel-item {*/
    /*padding: 0 130px 0 54px;*/
    /*position: relative;*/
  /*}*/
  /*.panel .panel-body .panel-item + .panel-item {*/
    /*padding-top: 56px;*/
    /*padding-bottom: 14px;*/
    /*margin-top: 50px;*/
    /*border-top: 1px solid #d4d4d4;*/
  /*}*/
  /*.panel .panel-body .panel-item .imgicon {*/
    /*position: absolute;*/
    /*left: 8px;*/
    /*top: 0;*/
    /*font-size: 36px;*/
  /*}*/
  /*.panel .panel-body .panel-item + .panel-item .imgicon {*/
    /*top: 62px;*/
  /*}*/
  /*.panel .panel-body .panel-item h4 {*/
    /*margin-bottom: 10px;*/
    /*font-size: 14px;*/
    /*color: #666;*/
    /*line-height: 1.3;*/
    /*font-weight: bold;*/
  /*}*/
  /*.panel .panel-body .panel-item p {*/
    /*font-size: 14px;*/
    /*color: #666;*/
    /*line-height: 1.3;*/
  /*}*/
  /*.panel .panel-body .panel-item .btn {*/
    /*padding: 0;*/
    /*width: 98px;*/
    /*height: 32px;*/
    /*text-align: center;*/
    /*line-height: 32px;*/
    /*position: absolute;*/
    /*right: 0;*/
    /*top: 10px;*/
    /*color: #a7b7da;*/
    /*border: 1px solid #d4d4d4;*/
    /*background: #f5f5f5;*/
    /*cursor: pointer;*/
    /*outline: none;*/
  /*}*/
  /*.panel .panel-body .panel-item + .panel-item .btn {*/
    /*top: 62px;*/
  /*}*/
  /*!*支付方式*!*/
  /*.payType-title {*/
    /*height: 46px;*/
    /*line-height: 46px;*/
    /*margin-top: 30px;*/
    /*font-size: 16px;*/
    /*border-bottom: 1px solid #d4d4d4;*/
  /*}*/
  /*!*暂无*!*/
  /*.notAvailable {*/
    /*text-align: center;*/
    /*height: 30px;*/
    /*line-height: 30px;*/
    /*margin: 30px 0;*/
    /*font-size: 16px;*/
  /*}*/

  /*朦版 修改昵称or密码*/
  .background-000 {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1001;
    background: #000;
    opacity: .6;
  }
  /*模态框*/
  .modify {
    margin: 80px auto;
    min-width: 560px;
    /*min-height: 500px;*/
    background: #FFF;
    position: fixed;
    top: 80px;
    left: calc(50% - 280px);
    overflow: hidden;
    z-index: 1002;
  }
  /*模态头部*/
  .modify .modify_title h3{
    height: 25px;
    margin: 60px 443px 20px 80px;
    font-size: 18px;
    color: #222;
  }
  .modify .modify_title .close {
    position: absolute;
    right: 80px;
    top: 54px;
    display: block;
    transition: all .6s;
    font-size: 32px;
    cursor: pointer;
  }
  .modify .modify_title .close:hover {
    transform:rotate(-180deg);
  }
  /*输入框*/
  .modify .modify_input_pwd {
    width: 400px;
    margin: 0 auto;
  }
  .modify .modify_input_pwd input {
    margin-bottom: 30px;
    padding-left: 12px;
    width: 400px;
    height: 48px;
    border: 1px solid #E1E1E1;
  }
  .modify .modify_input_pwd input:hover {
    border: 1px solid #FA322B;
  }
  .modify .modify_input_pwd input:focus {
    border: 1px solid #FA322B;
    outline: none;
  }
  /*模态框按钮*/
  .modify .confirm-close {
    padding: 5px 170px 30px;
  }
  .modify .confirm-close a {
    width: 103px;
    height: 39px;
    line-height: 40px;
    text-align: center;
    display: block;
    color: #a7b7da;
    border: 1px solid #d4d4d4;
    background: #f5f5f5;
    border-radius: 2px;
  }
  .modify .confirm-close .closeBtn {
    background: #CCC;
    border-radius: 2px;
    margin-right: 40px;
    border: .5px solid #ccc;
    color: #fff;
  }

  /*添加支付方式*/
  #payPop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,.6);
    z-index: 10;
  }
  .pay-pop {
    width: 500px;
    margin: 0 auto;
    padding-top: 40px;
  }
  .pay-pop-cont {
    padding: 20px 40px;
    background: #fff;
    border-radius: 6px;
  }
  /*头部*/
  .pay-head {
    font-size: 16px;
    padding-bottom: 10px;
    border-bottom: 1px solid #ebebeb;
    position: relative;
  }
  .pay-head>i {
    position: absolute;
    top: 0;
    right: 0;
    font-size: 22px;
    cursor: pointer;
    transition: all .6s;
  }
  .pay-head>i:hover {
    transform:rotate(-180deg);
  }
  /*内容*/
  .pay-list li {
    padding: 5px 15px;
    position: relative;
  }
  .pay-list li>span {
    font-size: 13px;
    line-height: 25px;
  }
  .pay-list li>input, .pay-list li>select {
    width: 100%;
    height: 35px;
    border: 1px solid silver;
    border-radius: 4px;
    outline: none;
    padding-left: 3%;
    box-sizing: border-box;
  }
  /*上传支付二维码*/
  .acc-prog {
    width: 100%;
    border: 1px dashed #dddee1;
    text-align: center;
    line-height: 28px;
    position: relative;
  }
  .acc-prog input[name="file"] {
    width: 100%;
    height: 100%;
    opacity: 0;
    position: absolute;
    left: 0;
    display: none;
  }
  .acc-cont-up {
    cursor: pointer;
  }
  .acc-cont-up>p {
    font-size: 12px;
    padding: 10px 0;
  }
  /*上传二维码后*/
  .prog-img {
    overflow: hidden;
    text-align: center;
    position: relative;
  }
  .prog-img>i {
    position: absolute;
    top: 0;
    right: 10px;
    cursor: pointer;
    font-size: 14px;
    color: #555;
  }
  .prog-img>img {
    height: 100px;
  }
  /*不可更改的input背景*/
  .readonly {
    background: #eee;
  }

  /*按钮*/
  .pay-but {
    text-align: center;
    padding-top: 20px;
  }
  .pay-but>button {
    width: 150px;
    height: 40px;
    border-radius: 4px;
    border: 1px solid silver;
    font-size: 15px;
    outline: none;
    cursor: pointer;
    margin: 0 13px;
    background: #fff;
  }
  .pay-but>.confirm {
    background: #4895ea!important;
    color: #fff;
    border: 1px solid #4895ea!important;
  }

</style>
